---
Практическая работа:
    Название: 'Администрирование сетевых сервисов.'
    Действие: 'Модуль четвертый'
---

**Время выполнения: 30 минут**

В данной практической работе Вы установите и настроите rsyslog.

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

### **Задача 1: Установка rsyslog**

1. Установка rsyslog на Srv1
```
apt install rsyslog
```
### **Задача 2: Настройка rsyslog**

1. Посмотрите конфигурацию rsyslog
```
/etc/rsyslog.conf
```
2. Настройка мониторинга DHCP

```
nano /etc/rsyslog.d/dhcpd.conf
```
```
local7.* /var/log/dhcpd.log
```
3. Проверка конфигурации
```
rsyslogd -N 1
```
4. Перезапустите службу
```
systemctl restart rsyslog.service
```
5. Отправьте тестовое сообщение
```
logger -p local7.debug demo
```
6. Проверьте, что тестовое сообщение
```
grep demo /var/log/* 2> /dev/null
```
### **Задача 3: Настройка DHCP**

1. Настройка мониторинга
```
nano /etc/dhcp/dhcpd.conf
```
Добавить строку
```
log-facility local7;
```
2. Проверка конфигурации
```
dhcpd -t
```
3. Перезагрузуть DHCP
```
systemctl restart isc-dhcp-server.service
```
4. Проверьте содержимое лог-файла
```
cat /var/log/dhcpd.log
```
### **Задача 4: Пересылка логов rsyslog**

1. Установка rsyslog на Srv2
```
apt install rsyslog
```

2. Настройка rsyslog
```
/etc/rsyslog.conf
```
3. Найдите строку #module(load="imudp") и раскомментируйте её, убрав в начале #.
   
4. Найдите строку #input(type="imudp" port="514") и раскомментируйте её, убрав в
начале #.

5. Найдите строку #module(load="imtcp") и раскомментируйте её, убрав в начале #.
   
6. Найдите строку #input(type="imtcp" port="514") и раскомментируйте её, убрав в
начале #.

7. Добавить
```
$template RemoteLogs,"/var/log/rsyslog/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
& ~

```
8. Проверка синтаксиса
```
rsyslogd -N 1
```
9. Перезапуск службы
```
systemctl restart rsyslog.service
```
10. Переключиться на Srv1

11. Создать файл
```
/etc/rsyslog.d/to-srv2.conf
```
```
*.info @@192.168.100.21:514
```
12. Проверка синтаксиса
```
rsyslogd -N 1
```
13. Перезапуск службы
```
systemctl restart rsyslog.service
```
14. Сгенерируйте тестовое сообщение.
```
logger testmsgsrv1
```
