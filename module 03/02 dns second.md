---
Практическая работа:
    Название: 'Администрирование сетевых сервисов.'
    Действие: 'Модуль 3'
---
# **Сервис DNS**
**Время выполнения: 120 минут**

В данной практической работе Вы настроите службу DNS.

Этапы создания стенда:

- Развернем первый сервер DNS

- Развернем второй сервер DNS

Имена пользователей и пароли:
```
Для denian
login: student 
password: 111
```
```
Для Windows
login: student 
password: password
```
### **Практическая работа**

### **Задача 1: Установка DNS**

1. Подключиться к srv1

2. Установка DNS
```
apt update
```
```
apt install bind9
```
3. Получение файла конфигурации
```
scp student@192.168.100.1:/etc/bind/named.conf.options /etc/bind/
```
4. Проверка
```
named-checkconf
```
```
systemctl restart named.service
```
5. Настройка DNS Second
```
nano  /etc/bind/named.conf.local
```
