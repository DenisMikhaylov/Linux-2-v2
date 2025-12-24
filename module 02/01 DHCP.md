---
Практическая работа:
    Название: 'Настройка DHCP'
    Действие: 'Модуль 2'
---
# **Настройка DHCP**
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

Для Windows
login: student
password: password
```

### **Практическая работа**



### **Задача 1: Установка и настройка DHCP на Gate**
Устновка DHCP

1. Залогиниться на Gate

2. Установка DHCP
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
INTERFACESv4="enp0s8"
```

4. Настройка Scope

```
gate# nano /etc/dhcp/dhcpd.conf

```
```
log-facility local7;
```
```
shared-network LAN1 {
  subnet 192.168.100.0 netmask 255.255.255.0 {
    range 192.168.100.110 192.168.100.128;
    option routers 192.168.100.1;
    option domain-name "corp.local";
    option domain-name-servers 192.168.100.20, 192.168.100.21;
    default-lease-time 600;
    max-lease-time 7200;
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
n – не преобразовывать IP-адреса в доменные имена
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
8. Получите настройки по протоколу DHCPv4. Подключиться к клиенту cll

```
dhclient -v enp0s3
```
```
ip a
```
```
ip r
```
```
cat /etc/resolv.conf
```
9. Проверьте связь с машинами gate, srv1 и srv2 с помощью команды ping. Проверка
связи должна пройти успешно.
```
ping -c4 192.168.100.1
```
```
ping -c4 192.168.100.20
```
```
ping -c4 192.168.100.20
```
10. Возвращаемся на gate. Статитска работы 
```
# dhcp-lease-list
# less /var/lib/dhcp/dhcpd.leases
# grep dhcp /var/log/syslog
```

Статистика работы 
```
# dhcpd-pools
# dhcpd-pools -l /var/lib/dhcp/dhcpd.leases -c /etc/dhcp/dhcpd.conf
```
11. Посмотрите логи службы.
```
grep dhcpd /var/log/syslog
```
12. Подключится к cll
```
dhclient -r
```
13.Возвращаемся на gate.
```
grep dhcpd /var/log/syslog
```

### **Задача : Создание резервации**

1. Открыть файл конфигурации 
```
nano /etc/dhcp/dhcpd.conf
```
2. Добавить перед описанием scope файла

Узнать mac адресс windows клиента
```
host <Имя компьютера windows> {
hardware ethernet <Windows mac адрес>;
fixed-address 192.168.10.112;
}
```
3. Проверка
```
dhcpd -t
```
4. Перезапуск службы DHCP

```
systemctl restart isc-dhcp-server.service
```
```
systemctl status isc-dhcp-server.service
```
5. Подключиться к Windows проверить полученный IP

6. Возвращаемся на gate. Статитска работы 
```
# dhcp-lease-list
# less /var/lib/dhcp/dhcpd.leases
```
Статистика работы 
```
# dhcpd-pools
# dhcpd-pools -l /var/lib/dhcp/dhcpd.leases -c /etc/dhcp/dhcpd.conf
```
7. Посмотрите логи службы.
```
grep dhcpd /var/log/syslog
```

