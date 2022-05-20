# MikrotikBlockDangerAddresses - скрипт блокировки опасных IP-адресов, с которых пытались произвести подключение к роутеру.

Скрипт предназначен для автоматической блокировки злоумышленников из внешних сетей, интересующихся устройствами Mikrotik.
Работа скрипта сводится к автоматическому формированию и блокировке списка подозрительных IP-адресов.
Список подозрительных IP-адресов формируется на основе: 1. записей журнала устройства, 2. установленных и работающих правил Firewall.
Настройка скрипта производятся путём правки значений локальных переменных вначале тела скрипта. Краткое описание переменных содержится в комментариях.
Первый запуск скрипта должен производиться вручную из окна терминала для контроля его работы. Запуск настроенного скрипта должен производиться периодически по шедуллеру.

https://forummikrotik.ru/viewtopic.php?p=85250#p85250
