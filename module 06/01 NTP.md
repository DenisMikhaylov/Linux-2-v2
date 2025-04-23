1. Локализация временной зоны

```
timedatectl
```
```
timedatectl list-timezones
```
2. Установка часового пояса
```
timedatectl list-timezones | column
```
```
timedatectl set-timezone Europe/Moscow
```

3. Установка времени

```
date +%T -s "11:14:00"

timedatectl set-time "2018-10-06 10:35:00"

```

4. Вывод времени в нужном формате

```
$ date

$ date -u

$ date "+%Y-%m-%d_%H:%M:%S"

$ date --date "1 day ago" +%Y-%m-%d

$ date --date "1 hour ago" "+%Y-%m-%d %H"
```

Настройка синхронизации с сервера gate
1. Переходим на SRV1

2. Проверяем синхронизацию
```
timedatectl
```
3. Настройка синхронизации
```
nano /etc/systemd/timesyncd.conf
```
```
[time]
NTP=gate.corp.ru

```
4. перезапуск службы

```
systemctl restart systemd-timesyncd

```
5. Проверяем синхронизацию
```
timedatectl
```

```
systemctl status systemd-timesyncd
```


Для Microsoft настройки производим в ручную или через GPO


1.Установка и настройка ntp сервера
Альтернативная служба.
```
gate:~# apt install ntp
```
2. Настройка
```
gate# nano /etc/ntpsec/ntp.conf
```
```
server 0.ru.pool.ntp.org
server 1.ru.pool.ntp.org
server 2.ru.pool.ntp.org
server 3.ru.pool.ntp.org
```

3. Перезапуск

```
gate# service ntp restart
```

Через 5-10 минут проверяем:

```
gate# ntpq -pn
```
```
*128.0.142.251  ...
+193.192.36.3   ...
+188.225.9.167  ...
```
```
gate# watch ntptrace

gate# apt install ntpstat

gate# ntpstat
```
```
gate# ntpdate time.apple.com
```
