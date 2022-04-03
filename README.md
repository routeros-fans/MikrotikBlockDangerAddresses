# MikrotikBlockDangerAddresses - скрипт блокировки опасных IP-адресов, с которых пытались произвести подключение к роутеру.

Скрипт создан для автоматической нейтрализации попыток завладения устройствами Mikrotik из внешней сети.
Работа скрипта сводится к выполнению двух основных задач: 
 1. Отслеживание журнала устройства и формирование чёрного списка IP-адресов.
 2. Добавление и проверка наличия заранее заданных правил Firewall.

Скрипт имеет ряд предопределенных переменных, содержимое которых пользователь может менять по своему усмотрению:
* 'extremeScan' - выбор режима сканирования журнала. Может принимать одно из двух значений: 'false' или 'true'. При задействовании этой опции ('true') скрипт блокирует все IP-адреса, найденные в строках журнала: "TCP connection established from xxx.xxx.xxx.xxx". При таком подходе существует вероятность блокировки нужных Вам IP-адресов, поэтому их заранее следует внести в белый список: "Firewall->Address Lists"->"WhiteList". В случае, когда эта опция не задействована ('false') - производится бОльшее количество проверок на наличие несанкционированного доступа к Вашему устройству, при этом не все попытки доступа к Вашему девайсу будут считаться несанкционированными. Таким образом, активирование данной опции разгружает устройство, но может блокировать валидные IP-адреса. Деактивирование опции нагружает устройство, но вероятность блокировки валидных IP-адресов отсутствует.
* 'timeoutBL' - время продолжительности блокировки. Задаёт время продолжительности блокировки, которое должно быть указано в единицах времени RouterOS. Например: '2w', '14d', '11d 11:00:01'.
'inIfaceList' - задает имя списка интерфейсов, через который роутер подключен во внешнюю сеть. Этот список живёт в: 'Interface->Interface List-> Lists' и требуется при добавлении новых правил в Firewall. Типовое название списка интерфейсов для выхода в интернет: 'internet', 'WAN' и т.п. (ВНИМАНИЕ!!! Регистр букв важен). В случае, если имя оставить пустым (""), тогда будет автоматически подставлено нужное имя списка из представленных. Нужно учитывать, что на данный момент автоподбор имени списка интерфейсов может работать не совсем корректно.
* 'nameBlackList', 'nameWhiteList' - задают названия для черного и белого списков в правилах Firewall.
* 'commentRuleBL', 'commentRuleWL' - задают комментарий для правил Firewall, работающих с чёрным и белым списками.

Скрипт следит как за списком правил Firewall Filter, так и за Firewall RAW. После установки правил Firewall пользователю необходимо расставить их в порядке по своему вкусу. Для исключения неожиданной блокировки при установке новых правил, правила блокировки устанавливаются ОТКЛЮЧЕННЫМИ (!!!). Перемещение и включение правил блокировки пользователь производит ВРУЧНУЮ, рекомендуется это делать в режиме Safe Mode, для того чтобы в случае потери связи с микротиком, когда режим Safe Mode отключился, все настройки вернулись как были. Важно не забыть отключить Safe Mode после настройки. Если правила блокировки оставить отключенными - скрипт будет оповещать об этом в LOG и терминал.

Запуск скрипта из окна терминала позволяет увидеть отчёт о его работе. Это настоятельно рекомендуется сделать при первом запуске для контроля работы скрипта. Запуск уже настроенного скрипта должен производиться по шедуллеру.

https://forummikrotik.ru/viewtopic.php?p=84017#p84017
