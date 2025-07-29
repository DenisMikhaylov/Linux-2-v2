Настройка Динамического обновления

Подключение к серверу Gate
```
nano /etc/dhcp/dhcpd.conf
```
```
ddns-updates on;
ddns-update-style interim;
ignore client-updates;
update-static-leases on;

ddns-ttl 60;

include "/etc/dhcp/rndc.key";
  zone corp.ru. {
    primary 192.168.10.1;
    key rndc-key;
  }
  zone 10.168.192.in-addr.arpa. {
    primary 192.168.10.1;
    key rndc-key;
  }
```
Проверка конфигурации
```
dhcpd -t
```
Настройка DNS
```
mv /etc/bind/corp* /var/cache/bind/
```
```
chown -R bind /var/cache/bind/
```
```
cp /etc/bind/rndc.key /etc/dhcp/
```
```
nano  /etc/bind/named.conf.local
```
```
...
        zone "corp.ru" {
...
               file "/var/cache/bind/corp.ru"
               allow-update { key "rndc-key"; };
        };
...
        zone "10.168.192.IN-ADDR.ARPA" {
...
               allow-update { key "rndc-key"; };
        };
...
include "/etc/bind/rndc.key";
```

Просмотр статистики обращений к DNS серверу
```
nano /etc/bind/named.conf.options
```
```
options{

  zone-statistics yes;
}
```
```
rndc stats
```
```
cat /var/cache/bind/named.stats
```
success - число запросов, не вызвавших ошибок или возврата клиенту ссылки
referral - число запросов, на которые сервер вернул клиенту ссылки
nxrrset - несуществующих записей запрошенного типа для доменного имени
nxdomain - несуществующих доменных имен
recursion - число запросов, потребовавших рекурсивной обработки
failure - число ошибок, кроме nxrrset и nxdomain
