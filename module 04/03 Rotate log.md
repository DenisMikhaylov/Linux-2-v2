1. Подключиться к Srv1

2. Настройка файл 
```
/etc/logrotate.d/dhcpd
```
```
/var/log/dhcpd.log
{
rotate 7
daily
missingok
notifempty
compress
delaycompress
postrotate
/usr/lib/rsyslog/rsyslog-rotate
endscript
}
```
, где
rotate 7 – хранить семь ротаций лог-файла;
daily – выполнять ротацию раз в сутки;
missingok – не считать ошибкой отсутствие изначального лог-файла;
notifempty – не выполнять ротацию, если файл есть, но пуст;
compress – сжимать ротированые лог-файлы с помощью gzip;
delaycompress – не сжимать первый ротированый лог-файл;
postrotate … endscript – выполнить указанные скрипты после ротации лог-файла.

3. Проверка расписания
```
systemctl list-timers
```
```
systemctl status logrotate.timer
```
```
systemctl cat logrotate.timer
```
