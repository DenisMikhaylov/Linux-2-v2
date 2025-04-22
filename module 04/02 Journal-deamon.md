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
4. За последжний час
```
journalctl -u named.service --since '2024-03-11 13:00' --until '2024-03-11 13:59:59'
```
5.  Посмотрите логи DHCP с помощью journalctl
```
journalctl -u isc-dhcp-server.service
journalctl -u isc-dhcp-relay.service
```
