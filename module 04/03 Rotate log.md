---
Практическая работа:
    Название: 'Администрирование сетевых сервисов.'
    Действие: 'Модуль четвертый'
---

**Время выполнения: 30 минут**

В данной практической работе Вы  настроите logrotate.

Этапы создания стенда:

- Импорт виртуальных машин

- Настройка виртуальных машин и сети

- Развертывания сервисов

Имена пользователей и пароли:
```
Для denian
login: student 
password: 111
```
```
Для Windows
login: Admin 
password: Pa55w.rd
```
### **Практическая работа**

### **Задача 1: Настройка logrotate **

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
