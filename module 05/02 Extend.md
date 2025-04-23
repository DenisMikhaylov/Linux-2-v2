Скрипт для настройки шлюза
```
# nano firewall.sh
```
```
iptables --flush

iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.20 --dport 22 -j ACCEPT
iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.20 --dport 53 -j ACCEPT
iptables -A FORWARD -i enp0s3 -p udp -d 192.168.100.20 --dport 53 -j ACCEPT
#iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.1 --dport 25 -j REJECT
#iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.1 --dport 25 -j ACCEPT
#iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.1 --dport 465 -j ACCEPT
#iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.1 --dport 587 -j ACCEPT
#iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.1 --dport 143 -j ACCEPT
iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.1 --dport 80 -j ACCEPT
iptables -A FORWARD -i enp0s3 -p tcp -d 192.168.100.1 --dport 5222 -j ACCEPT


iptables -A FORWARD -i enp0s3 -s 192.168.100.0/24 -j ACCEPT
iptables -A FORWARD -i enp0s3 -s 192.168.110.0/24 -j ACCEPT


iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT

iptables -A FORWARD -j DROP

conntrack -F
```
```
# apt install conntrack

# sh firewall.sh

# iptables-save > /etc/iptables.rules
```

Управление состоянием iptables

Вариант 1

Сохранение состояния iptables
```
gate:~# iptables-save > /etc/iptables.rules
```
Восстановление состояния iptables
```
gate:~# iptables-restore < /etc/iptables.rules
```
Восстановление состояния iptables при загрузке
```
gate:~# nano /etc/network/interfaces
```
Добавить в конец файла
```
  pre-up iptables-restore < /etc/iptables.rules
```
Вариант 2
```
# apt install iptables-persistent
```
```
# netfilter-persistent save
```



