# MikrotikBlockDangerAddresses - скрипт блокировки опасных IP-адресов, с которых пытались произвести подключение к роутеру.

Скрипт предназначен для автоматической блокировки злоумышленников из внешних сетей, сканирующих маршрутизаторы Mikrotik. 
Работосопособность скрипта проверялась только на актуальных версиях RouterOS 6.49.++ и 7.14.++
Код скрипта содержит в себе всё необходимое для работы и не имеет зависимостей от сторонних функций и скриптов.
Настройка скрипта сводится к правке значений локальных переменных в начале тела скрипта, краткое описание которых имеется в комментариях.
Скрипт необходимо добавить в System/Scripts и запустить вручную из окна терминала, для чтения отчёта о состоянии и внесения необходимых правок в настройки. 
Запуск настроенного скрипта должен производиться по расписанию System/Scheduler с необходимым периодом (типовое значение обычно составляет единицы минут).

Работа скрипта сводится к формированию чёрного списка IP-v4-адресов для их последующей блокировки.
Формирование чёрного списка происходит двумя разными независимыми способами:
  1. на основе анализа записей журнала устройства
  2. при помощи преднастроенных правил Firewall

1й способ срабатывает каждый раз при запуске скрипта.

2й способ изначально отключен, это сделано из соображения, что Firewall может быть уже настроен и вмешательство в его работу не желательно.
Активация 2го способа производится вручную, путём присвоения переменной 'firewallUsage' значения 'true'.
После активации 2го способа скрипт настраивает Firewall по принципу: "запрещено всё, что не разрешено" и с этого момента, даже если скрипт не запущен, все попытки из вне прощупать роутер будут считаться несанкционированными.
Попутно, при задействовании 2го способа, при каждом запуске скрипт проверяет наличие преднастроенных правил Firewall и в случае их отсутствия производит установку недостающих. Поиск недостающих правил Firewall скрипт производит по комментариям в списках: 'Firewall/Filter Rules', 'Firewall/Raw', 'Firewall/Layer7 Protocols'. После установки правил Firewall пользователю необходимо расположить их вручную согласно своим предпочтениям. Для исключения неожиданной блокировки при установке новых правил, правила блокировки устанавливаются ОТКЛЮЧЕННЫМИ (!!!). Перемещение и включение правил блокировки пользователь производит ВРУЧНУЮ, рекомендуется это делать в режиме 'Safe Mode', для того чтобы в случае потери связи с роутером, когда режим 'Safe Mode' отключился, все настройки вернулись как были. Важно не забыть отключить 'Safe Mode' после настройки. Если правила блокировки оставить отключенными - скрипт будет оповещать об этом.

Таким образом, все обращения к роутеру, не прошедшие проверку Firewall или отражённые в журнале устройства, как попытки несанкционированного доступа, попадают в черный список и временно блокируются. Типовое количество записей в чёрном списке при блокировке на 8 часов может составлять от нескольких сотен до нескольких тысяч (!!!) записей.

Известные проблемы:
* работа скрипта может завершаться ошибкой, при включенном контроле правил Firewall и наличии в списке одинаковых правил с отличающимися комментариями.
* для предотвращения блокировки скриптом адресов, влияющих на работоспособность сетевого оборудования (DNS провайдера, адрес вышестоящего узла и т.п.), имеет смысл заранее внести их в белый список.

https://forummikrotik.ru/viewtopic.php?p=91125#p91125
