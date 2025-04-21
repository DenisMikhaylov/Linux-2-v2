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

### **Задача 1: Диагности работы DNS с Gate**

1. Подключиться к Gate

2. Диагности DNS

```
nslookup -q=A ya.ru
nslookup -q=AAAA ya.ru
nslookup -q=MX ya.ru
nslookup -q=TXT ya.ru
nslookup -q=SOA ya.ru
nslookup -q=NS ya.ru
```


3. Установка пакета
```
apt search '^bind9'
```

```
gate:~# apt install bind9 -y
```
4. Проверка информации
```
grep bind /etc/passwd
```
```
grep bind /etc/group
```
```
ls /var/cache/bind
```
```
systemctl status bind9
```
Проверка состовляемой конфигурации
```
gate# cat /etc/bind/named.conf
```
5. Настройка DNS
Настройка сервера перенаправляющего запросы на DNS cервер провайдера

```
# nano /etc/bind/named.conf.options
```
```
        forwarders {
                8.8.8.8;
        };
       dnssec-validation no;
       listen-on { 192.168.100.1; 127.0.0.1; };
       listen-on-v6 { ::; };
       allow-recursion {
          127.0.0.0/8;
          192.168.100.0/24;
          192.168.110.0/24;
      }; 
       

```
6. Проверка конфигурации

```
# named-checkconf -z
```

7. Рестарт службы DNS

```
systemctl restart bind9
```

8. Проверка портов
```
ss -pna4 | grep named

```

9. Тестирование пересылки

```
nslookup -1=A ya.ru 127.0.0.1
```

10. Правим настройки DNS
```
nano /etc/resolv.conf
```
```
nameserver 127.0.0.1
```
```
nslookup -1=A ya.ru 127.0.0.1
```

11. Настройка Мастер сервера

```
nano /etc/bind/named.conf.local
```
```
zone "corp.local" {
        type master;
        file "/etc/bind/corp.local";
};
```

12. создание файла zone
```
nano /etc/bind/corp.local
```
```
$TTL    3h
@    IN  SOA gate   root 2024041601 1d 12h  1w  3h
         NS  gate
         NS  srv1
         A   192.168.100.1
         MX  10  gate
gate     A   192.168.100.1
srv1     A   192.168.100.20


```

13. Проверка DNS zano

```
named-checkconf -z
named-checkzone corp.local /etc/bind/corp.local
```

14.Перезапуск DNS

```
rndc reload
```
```
systemctl reload named.service
```

15. Тестирование

```
nslookup -q=SOA corp.local
```
```
nslookup -q=A gate.corp.local
```
```
nslookup -q=NS corp.local
```
```
nslookup -q=NS srv2.corp.local
```

16. Добавление записей типа A
```
nano /etc/bind/corp.local
```
```
srv2  A  192.168.100.21
```
17. Проверка
```
named-checkconf -z
```
```
systemctl reload named.service
```
### **Задача 2: Обратная зона**

1. Создание обратной zone
```
nano /etc/bind/named.conf.local
```
2. Добавить в конец файла
```
zone "100.168.192.in-addr.arpa" in {
  type master;
  file "/etc/bind/100.168.192.IN-ADDR.ARPA";
};
```
3. Создание файла обратной зоны
```
nano /etc/bind/100.168.192.IN-ADDR.ARPA
```
```
$TTL    3h
@  SOA gate.corp.local.  root.corp.local. 2024040501 1d 12h  1w  3h
@  NS  gate.corp.local.
@  NS  srv1.corp.local.

1    PTR  gate.corp.local.
20   PTR  srv1.corp.local.
21   PTR  srv2.corp.local.

```
```
named-checkconf -z

systemctl restart bind9
```
