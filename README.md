1. Смонтируйте удаленную директорию на аккаунте хостинга в директорию на локальном компьютере (воспользуйтесь sshfs).

Командая для монтирования:
```
sshfs stlark@stlark.beget.tech:/home/s/stlark/ ./test
```

Вывод домашней директории пользователя `stlark` на сервере виртуального хостинга
```
stlark@stlark-GF63-Thin-9SCSR:~$ ls -la test/
итого 156
drwx------  1 root   root    4096 дек  9 15:26 .
drwxr-x--- 80 stlark stlark  4096 дек 11 09:38 ..
drwx------  1 root   root    4096 дек  9 14:33 admin.stlark.ru
-rw-------  1   4352    601   215 дек  9 14:35 .bash_history
-rwx------  1   4352    601   119 авг 27 00:21 .bashrc
drwx------  1   4352    601  4096 ноя 16 03:00 .beget
-rw-------  1   4352    601 13828 ноя 29 12:06 beget_mailtest_smtp.php
drwx------  1   4352    601  4096 ноя  2 14:56 .cache
drwx------  1   4352    601  4096 ноя  7 00:48 cmsupdate
-rw-------  1   4352    601   168 дек  9 14:35 .gitconfig
drwx------  1 root   root    4096 ноя 10 09:54 joomla.stlark.ru
-rw-------  1   4352    601  1729 ноя 29 15:17 mail328.php
-rw-------  1   4352    601  2803 дек  9 15:26 .mysql_history
drwx------  1 root   root    4096 ноя  9 17:56 ocstore302.stlark.ru
drwx------  1 root   root    4096 ноя  2 15:08 ocstore.stlark.ru
drwx------  1 root   root    4096 ноя  7 01:24 oldpresta.stlark.ru
drwx------  1 root   root    4096 ноя  2 15:43 opencart.stlark.ru
drwx------  1 root   root    4096 ноя  8 13:01 own.stlark.ru
drwx------  1 root   root    4096 ноя  2 23:37 prestashop.stlark.ru
dr-x------  1 root   root    4096 сен  2 15:41 .service
drwx------  1   4352    601  4096 дек  9 14:33 .ssh
drwx------  1 root   root    4096 ноя  6 23:06 stlark.ru
-rw-------  1   4352    601    11 ноя 10 17:39 test
-rw-------  1   4352    601    12 ноя 10 17:39 test1
-rw-------  1   4352    601   681 дек  1 15:41 test.py
drwx------  1 root   root    4096 ноя  6 22:12 tjoomla.stlark.ru
drwx------  1 root   root    4096 ноя  9 18:26 tocstore.stlark.ru
drwx------  1 root   root    4096 ноя  7 00:55 tpresta.stlark.ru
drwx------  1   4352    601  4096 ноя  8 15:08 .vim
-rw-------  1   4352    601 22213 дек  9 14:34 .viminfo
-rw-------  1   4352    601   302 дек  8 00:27 .wget-hsts
stlark@stlark-GF63-Thin-9SCSR:~$ 
```

2. Клиент попросил развернуть сайт http://cp.beget.com/shared/lVtkDsffGGjOb_-29LegqF1WdFUY0jgR/backup.tar.gz, вам нужно помочь ему. Сайт нужно развернуть на личном аккаунте с тарифом виртуального хостинга (если аккаунта нет, то создайте с тестовым периодом).

Развернутый сайт доступен по домену admin.stlark.ru

Тестовая запись открывается

![изображение](https://github.com/st1lark/admin_test/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202022-12-11%2010-51-22.png)

Досутпы в админку:
```
asdsaasd
kB0EWEDCSs
```

Скришот админки:

![](https://github.com/st1lark/admin_test/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202022-12-11%2010-56-11.png)
