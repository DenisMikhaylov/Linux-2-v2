---
Практическая работа:
    Название: 'Администрирование сетевых сервисов.'
    Действие: 'Модуль четвертый'
---

**Время выполнения: 30 минут**

В данной практической работе Вы будите использовать journalctl.

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

### **Задача 1: Просмотр лога **
1. Подключиться к Gate
2. Проверьте наличие каталога /var/log/journal
```
ls -ld /var/log/journal
```
3. Посмотрите логи DNS-сервера bind с помощью journalctl
```
journalctl -u named.service
```
Посмотреть uid bind
```
journalctl _UID=<uid bind>
```
Фильтрация по уровням важности сообщений.
```
journalctl -p 5
```
Просмотр логов с момента последней загрузки
```
journalctl --boot
```
Просмотр логов процесса с PID 
```
journalctl _PID=<pid bind>
```
4. За последний час
```
journalctl -u named.service --since '2024-03-11 13:00' --until '2024-03-11 13:59:59'
```
5.  Посмотрите логи DHCP с помощью journalctl
```
journalctl -u isc-dhcp-server.service
```
```
journalctl -u isc-dhcp-relay.service
```
