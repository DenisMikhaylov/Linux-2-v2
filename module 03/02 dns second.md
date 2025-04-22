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

### **Задача 1: Установка slave DNS**

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
6. Открываем настройки

```
nano /etc/bind/named.conf.local
```
```
zone "corp.local" {
        type slave;
        file "/var/cache/bind/corp.local";
        masterfile-format text;
        masters { 192.168.100.1; };
};
zone "100.168.192.in-addr.arpa" in {
        type slave;
        file "/var/cache/bind/100.168.192.IN-ADDR.ARPA";
        masterfile-format text;
        masters { 192.168.100.1; };
};
zone "110.168.192.in-addr.arpa" in {
        type master;
        file "/var/cache/bind/110.168.192.IN-ADDR.ARPA";
        masterfile-format text;
        masters { 192.168.100.1; };
};

```

7. Перезапускаем и проверяем bind

```
named-checkconf -z
```
```
systemctl restart bind9
```
8. Провыеряем создание файлов

```
ls /var/cache/bind
```
9. Должен появиться фаил с нашей зоной

```
cat /var/cache/bind/corp.local
```

10. Настройки на себя DNS
```
nano /etc/resolv.conf
```
```
nameserver 127.0.0.1
```
11. Проверка работы
```
journalctl --unit named

```
12. Проверка домашнего каталога пользователя
```
ls ~bind
```
### **Задача 2: Проверка с помощью DIG**
    
1. Проверка с помощьб dig
```
dig SOA corp.local
```
```
dig NS corp.local
```
```
dig A gate.corp.local
```
```
dig A srv1.corp.local
```
```
dig PTR 1.100.168.192.in-addr.arpa
```
### **Задача 3: Проверка уведомлений от Master**

1. Настройка дополнительной записи на Gate
```
namo /etc/bind/corp.local
```
```
Добавить запись www для srv2
```

2. Проверка конфигурации
```
named-checkconf -z
```
3. Перзагрузка сервиса
```
systemctl reload named.service
```
4. Проверка записи
```
nslookup -q=A www.corp.local
```
5. Анализ журнала
```
journalctl -unit named
```
6. Переключиться на srv1
7. Проверка описание зоны
```
cat ~bind/corp.local
```
### **Задача 4: Утилита RNDC**

1. Получение статуса
```
rndc status
```

2. Сохраните кэш службы в файл.
```
rndc dumpdb
```
3. Посмотр кэш.
```
less ~bind/named_dump.db
```
7. Очистите кэша.
```
rndc flush
```
8. Включиние протоколирование запросов.
```
rndc querylog on
```
```
nslookup -q=A srv1.corp.local
```
9. Просмотр лога
```
journalctl -unit named
```
10. Выключение протоколирование запросов
```
rndc querylog off
```
 
