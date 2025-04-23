Установка на Хост WinSCP

Подключиться к Gate
Добавить пользователя
```
useradd student -m
```
пароль Pa$$w0rd

Скачать и установить WinSCP на хост

```
https://winscp.net/eng/download.php
```
Запускаем утилиту WinSCP

SSH предварительно должен быть настроен на серверах LINUX

Подключение SCP

Вибираем протокол SCP
Имя хоста 192.168.100.1 
порт по умолчанию
Пользователь student
пароль Pa$$w0rd

Проходим по файловой системе и можем скопировать файл на хост

Установка ProFTP

Переходим на Gate

Устанавливаем
```
# apt install proftpd-basic
```
Устанавливаем консольного клиента

```
apt install ftp
```

Просмотреть список ftp
```
ftpwho
```

Подключаемся к сами себе

```
ftp 127.0.0.1
```
Пользователь student
пароль Pa$$w0rd

Проверка работы прохождения по каталогам
```
ls
cd ..
ls
cd ..
ls
```
```
quit
```

Ограничение пользователя каталогами

```
nano /etc/proftpd/proftpd.conf
```
Расскоментируем строку
```
DefaultRoot ~
```
Проверка файла конфигурации
```
proftpd -t
```

Перезапускаем службу 

```
systemctl restart proftpd
```

Настройка анонимного доступа

```
nano /etc/proftpd/proftpd.conf
```
Расскоментируем строку
```
<Anonymous ~ftp>
   User                         ftp
   UserAlias                    anonymous ftp
   RequireValidShell            off
</Anonymous>
```
Перезапускаем службу 

```
systemctl restart proftpd
```

Подключаемся к сами себе

```
ftp 127.0.0.1
```
Проверка работы прохождения по каталогам
```
ls
cd ..
ls
cd ..
ls
```
```
quit
```

Сокрытие названия/версии сервиса

```
nano /etc/proftpd/proftpd.conf
```

```
ServerIdent on "MS Ftp Server"
```
Перезапускаем службу 

```
systemctl restart proftpd
```

FTP TLS

```
nano /etc/proftpd/proftpd.conf
```
```
Include /etc/proftpd/tls.conf
```
Нaстройка модуля TLS

```
nano /etc/proftpd/tls.conf
```
```
TLSEngine on
TLSLog /var/log/proftpd/tls.log
TLSProtocol SSLv23
TLSOptions NoSessionReuseRequired
TLSRSACertificateFile /etc/proftpd/ssl/proftpd.cert.pem
TLSRSACertificateKeyFile /etc/proftpd/ssl/proftpd.key.pem
TLSVerifyClient off
TLSRequired on
```
Создание сертификата
```
apt install  openssl
```
```
mkdir /etc/proftpd/ssl
```
Генерируем SSL сертификат
```
openssl req -new -x509 -days 365 -nodes -out \
/etc/proftpd/ssl/proftpd.cert.pem -keyout \
/etc/proftpd/ssl/proftpd.key.pem
```
Вводим вашу регистрационную информацию
```
Country Name (2 letter code) [AU]: RU
State or Province Name (full name) [Some­State]: Moscow
Locality Name (eg, city) []: Moscow
Organization Name (eg, company) [Internet Widgits Pty Ltd]: CLASS
Organizational Unit Name (eg, section) []: IT
Common Name (eg, YOUR name) []: corp1.ru
Email Address []: root@localhost
```
Перезапускаем службу 

```
systemctl restart proftpd
```
Установка FTP-ssl

```
apt install ftp-ssl
```



подключаемся к серверу соседа:
```
ftp-ssl 127.0.0.1
```
В случае возникновения проблем с TLS смотрите логи /var/log/proftpd/tls.log






