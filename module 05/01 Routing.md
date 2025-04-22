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

20. Дальнейшие сохранниния можно сделать следующей командой

```
iptables-save > /etc/iptables/rules.v4
```

21. Блокируем внешние запросы

```
iptables -A INPUT -i enp0s3 -p udp --dport 53 -j DROP
```

Можно проверить теперь работу DNS коллег указывая из ip eth0 от Gate
```
nslookup -q=A corp.ru <ip address>
```
```
iptables -L
```

Сmотрим номер правило
```
iptables --line-numbers -t nat -L

```
смотрим номер правила

```
iptables -D INPUT <ввести номер правила>
```
Восстановите правило использую iptables-restore
Добавить правило

вместо <IP enp0s8> ваш внешний ip gate
```
iptables -A INPUT -i eth0 -p udp --dport 22 -j DROP
iptables -t nat -A PREROUTING -i eth0 --destination <IP ETH0> -p tcp --dport 2222 -j DNAT --to-destination 192.168.10.10:22
iptables -t nat -A PREROUTING -i eth0 --destination <IP ETH0> -p tcp --dport 2221 -j DNAT --to-destination 192.168.10.1:22
```

