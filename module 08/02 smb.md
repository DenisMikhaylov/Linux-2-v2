Настройка Samba
1. Подключаемся к srv1
2. Установка SAMBA
```
# apt install samba
```
3. Проверяем порты
```
ss -pna4 | grep smbd
```
4. Создание папrb для share
```
mkdir /var/smb
```
5. Назначаем права на папку
```
chmod -R a=rwX /var/smb
```

6. Создаем полльзователя для доступа к папкам

```
useradd smbuser -s /usr/sbin/nologin
```
```
passwd smbuser
```
or
```
adduser Administrator
```
```
adduser student
```
пароль Pa$$w0rd

Создаем пользователя для samba
```
smbpasswd -a smbuser
smbpasswd -a Administrator
```
пароль Pa$$w0rd


Редактируем настройки

```
nano /etc/samba/smb.conf
```
добавляем в конец
```
[share1]
   path = /var/smb
   guest ok = yes
   read only = no
   force user = smbuser
```


Проверяем 

```
testparm
```
наживаем enter
и смотрим настройки

Проверяем
переходим на Windows 

```
//192.168.100.20/share1
```

Можно с хоста по внешнему ip gate

может не подключится


Создаем папку не для всех пользователей

Редактируем настройки

```
nano /etc/samba/smb.conf
```
добавляем в конец
```
[global]
   guest account = nobody
[share2]
   path = /var/share2
   guest ok = yes
   public = yes
   writable = yes
   available = yes
```
Создаем каталог

```
mkdir /var/share2
chown nobody:nogroup /var/share2
chmod 0775 /var/share2
ls -la /var/share2
```

Проверяем 

```
testparm
```
наживаем enter
и смотрим настройки

Проверяем
переходим на Windows 

```
//192.168.100.20/share2
```


Помощь

```
man 5 smb.conf
```

может не подключится
Открываем в Microsoft powershell

```
net use * \\192.168.100.20\share1 /USER:smbuser
```


Сокрытие названия/версии сервиса
```
# nano /etc/samba/smb.conf
```
```
[global]
...
  server string = MS File Server

...
```

Переключаемся на SRV2
Автоматическое монтирование Samba на linux

Устанавливаем Cifs

```
apt install samba-client cifs-utils -y
```

редактируем Fstab

```
nano /etc/fstab
```

```
//srv1.corp.local/share1 /home/student/samba cifs defaults,user=smbuser,noauto 0 0
```
Создается пользователя student и каталог /home/student/samba

```
mount /home/student/samba
```


