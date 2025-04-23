# Настройка NFS
Подключаемся к серверу srv1

1. Установка NFS сервера

```
apt install nfs-kernel-server -y
```

2. Просмотреть настройки

```
nano /etc/exports
```



3. Настроим доступ до каталога

```
mkdir /var/nfs
```
```
ls -la /var/nfs
```
4. Настроить права на запись

```
chmod 777 /var/nfs
```

5. Настройка доступа

```
nano /etc/exports
```
6. Добавлем строку

```
/var/nfs  cll.corp.local(rw,sync,no_subtree_check)
/var/nfs  srv2.corp.local(rw,sync,no_subtree_check)
```

7. Перезапускаем сервис

```
systemctl restart nfs-kernel-server
```
or
```
service nfs-kernel-server force-reload
```
8. Проверка NFS
```
showmount -e
```

Переключаемся на SRV2

1. Устанавливаем клиента NFS

```
# apt install nfs-common
```

3. Сервис в дебиане может быть быть выключен и замаскирован

```
ls -l /lib/systemd/system/nfs-common.service
```
4. Проверяем что есть /dev/null

5. Удалем файл
```
rm /lib/systemd/system/nfs-common.service
```
6. Запускаем сервис

```
systemctl enable nfs-common
systemctl start nfs-common
```
7. Смотрим доступные ресурсы

```
showmount -e srv1.corp.local
```
8. Монтирование каталога
```
# mkdir /mnt/nfs
# mount srv1.corp.local:/var/nfs /mnt/nfs
```
9. Добавим в автозагрузку
```
# nano /etc/fstab
```
```
192.168.100.20:/var/nfs  /mnt/nfs nfs4 defaults 0 0 
```

10. Примонтируем
```
mount /mnt/nfs
```


Для правильной настройки лучше использовать autofs

1. Установка autofs
```
# apt install autofs
```
2. Редактируем файл
```
nano /etc/auto.master 
```
3. Добавляем в конец
```
/- /etc/auto.misc
```
```
nano /etc/auto.misc
```
```
/mnt/nfs  -fstype=nfs4,rw 192.168.100.20:/var/nfs

```
```
service autofs restart
```
4. Проверяем

Создаем файл в папке /var/nfs на srv1 с названием test

на srv2 переходим в папку /mnt/nfs и проверяем налицие файла test

