Настройка маршрута

1. Подключаемся к gate

2. Настраиваем перенаправление ip
```
nano /etc/sysctl.conf
```
3. Правим строку
```
net.ipv4.ip_forward=1
```
4. Перечитаем конфигурацию

```
sysctl -f
```

5. Проверяем

```
sysctl net.ipv4.ip_forward
```
6. Проверка таблицы маршрутов

```
ip r
```

7. Настройка и установка ip forward

8. Установка iptables
```
# apt install iptables conntrack -y
```

9. Отобразится таблица filter
```
iptables -L
```
10. Отобразится таблица nat

```
iptables -t nat -L
```
11. Переходим на srv1

12. проверяем работу
```
ping -c4 ya.ru
```
ответа не будет. так как пакет не вернулся. но был отправлен.

13. Добавлем правило на gate

```
iptables -t nat -A POSTROUTING -o enp0s3 -s 192.168.100.0/24 -j MASQUERADE
```
14. Проверяем

```
iptables -t nat -L
```

15. Сохранение или просмотр iptables

15.1. Просмотреть
```
iptables-save
```
15.2. сохранить 
```
iptables-save > testiptables
```

16. Переходим на srv1

17. проверяем работу
```
ping -c4 ya.ru
```

18. Переключаемся на gate

19. Устанавливаем пакет для загрузки настроек iptables
```
apt install iptables-persistent -y
Yes
Yes
```

20.Добавление изминений

```
iptables-save > /etc/iptables/rules.v4
```

21. Блокируем внешние запросы

```
iptables -A INPUT -i enp0s3 -p udp --dport 53 -j DROP
```

22. Можно проверить теперь работу DNS коллег указывая из ip enp0s3 от Gate
```
nslookup -q=A corp.local <ip address>
```
```
iptables -L
```

23. Сmотрим номер правило
```
iptables --line-numbers -L

```
24. смотрим номер правила

```
iptables -D INPUT <ввести номер правила>
```
25. Восстановите правило использую iptables-restore
26. Добавить правило

```
iptables -A INPUT -i enp0s3 -p udp --dport 22 -j DROP
```
```
iptables -t nat -A PREROUTING -i enp0s3--destination <ip enp0s3> -p tcp --dport 2222 -j DNAT --to-destination 192.168.100.20:22
```
```
iptables -t nat -A PREROUTING -i enp0s3 --destination <ip enp0s3> -p tcp --dport 2221 -j DNAT --to-destination 192.168.100.21:22
```
```
iptables -t mangle -A POSTROUTING -j TTL --ttl-set 65
```
27. Правило для просмотр соединения
```
iptables -I FORWARD 1 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```
Просмотреть правило
```
iptables -L FORWARD --line
```
28. Проверка пинг. Подключиться к srv1
```
ping gate.corp.local
```
Вернуться на gate
```
conntrack -L -p icmp
```
29. Правило запись в лог
```
iptables -I FORWARD 2 -i enp0s8 -o enp0s9 -s 192.168.100.0/24 -d 192.168.110.0/24 -p icmp -j LOG --log-prefix "Demo "
```
Провести пинг cll
```
Ping cll
```
Проверка лога
```
grep Demo /var/log/syslog
```
```
grep Demo /var/log/kern.log
```
30. Запрет пинг в сеть 192.168.110.0/24
```
iptables -A INPUT -i enp0s9 -s 192.168.100.0/24 -p icmp -j DROP
```
31. 
