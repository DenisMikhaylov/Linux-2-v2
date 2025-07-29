---
Практическая работа:
    Название: 'Настройка DHCP'
    Действие: 'Модуль 2'
---
# **Настройка DHCP relay**
**Время выполнения: 60 минут**

В данной практической работе вы установите и настроите сервис DHCP и ретранслятор DHCP .

Этапы:

- Установка и настройка DHCP

- Установка и настройка DHCP ретранслятора


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



### **Задача 1: Установка и настройка DHCP на Gate**
Устновка DHCP

1. Подключаемся к Gate
   
2. Остановите службу.
```
systemctl stop isc-dhcp-server.service
```
3. Удалите службу из автозагрузки.
```
systemctl disable isc-dhcp-server.service
```
4. Настройка пересылки пакетов
```
nano /etc/sysctl.conf
```
```
net.ipv4.ip_forward=1
```
5. Включение NAT
```
apt install iptables
```
```
iptables -t nat -A POSTROUTING -o enp0s3 -s 192.168.100/24 -j MASQUERADE
```
6. Добавить сетевой интерфейс к Gate
   
7. Добавить настройки сети
```
#LAN1
auto enp0s9
iface enp0s9 inet static
address 192.168.110.1/24  
```
8. Перезагрузить службу сети
```
systemctl restart networking
```
```
ip a
```

10. Залогиниться на srv

11. Настройка DNS сервера
```
nano /etc/resolv.conf
```
```
search corp.local
nameserver 8.8.8.8
```
9. Проверка работы сети
```
ping ya.ru
```
10. Установка DHCP
```
apt update
```
```
apt search '^isc-dhcp'
```
Во время установки игнорируйте сообщения об

ошибках запуска сервиса. После установки система заносит службу в

«автозагрузку» и пытается запустить её. Но конфигурация у службы тестовая, что

приводит к невозможности запуска и сообщениях об ошибках.

```
apt install isc-dhcp-server
```

3. Настройка Интерфейса

```
nano /etc/default/isc-dhcp-server
```
```
INTERFACESv4="enp0s3"
```

4. Настройка Scope

```
gate# nano /etc/dhcp/dhcpd.conf

```
```
log-facility local7;

shared-network LAN1 {
  subnet 192.168.100.0 netmask 255.255.255.0 {
    range 192.168.100.110 192.168.100.128;
    option routers 192.168.100.1;
    option domain-name "corp.local";
    option domain-name-servers 192.168.100.1, 192.168.100.20;
    default-lease-time 600;
    max-lease-time 7200;
  }
 shared-network LAN2 {
  subnet 192.168.110.0 netmask 255.255.255.0 {
    range 192.168.110.110 192.168.110.128;
    option routers 192.168.110.1;
    option domain-name "corp.local";
    option domain-name-servers 192.168.110.1, 192.168.100.20;
    default-lease-time 600;
    max-lease-time 7200;
  }
}

}
```

4. Проверка DHCP

```
# dhcpd -t
```

5. Перезапуск службы DHCP

```
systemctl restart isc-dhcp-server
```
6. Проверка работы DHCP
```
systemctl status isc-dhcp-server
```
7. Мониторинг работы DHCP

Прослушиваемые порты
```
где
p – показывать имя программы и PID;
n – не преобразовывать IP-адреса в доменные имена;
a – показать все порты (не только активные, но и прослушиваемые);
u – показать порты UDP;
4 – показать порты только для IPv4
```
```
ss -pnau4
```
```
# dhcp-lease-list
# less /var/lib/dhcp/dhcpd.leases
# grep dhcp /var/log/syslog
```

Статитска работы 
```
# apt install dhcpd-pools
# dhcpd-pools
# dhcpd-pools -l /var/lib/dhcp/dhcpd.leases -c /etc/dhcp/dhcpd.conf
```
8. Получите настройки по протоколу DHCPv4. Подключиться к клиенту cll изменить параметры сетевого интерфейса на "внутренняя сеть"

```
dhclient -v enp0s3
```

10. Возвращаемся на gate. Статитска работы 
```
# dhcp-lease-list
# less /var/lib/dhcp/dhcpd.leases
# grep dhcp /var/log/syslog
```
11. Подключитьна на gate
    
12. Установка пакета relya
```
apt install isc-dhcp-relay
```
13. Настройка
```
nano /etc/default/isc-dhcp-relay
```
```
SERVERS="192.168.100.20"
OPTIONS="-iu enp0s8 -id enp0s9"
```

14. Перезапустить службу
```
systemctl restart isc-dhcp-relay.service
```

15. Подключится к cll
```
dhclient -v enp0s3
```
```
ip a 
```



