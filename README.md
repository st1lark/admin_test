**1. Смонтируйте удаленную директорию на аккаунте хостинга в директорию на локальном компьютере (воспользуйтесь sshfs).**

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

--------------------------------

**2. Клиент попросил развернуть сайт http://cp.beget.com/shared/lVtkDsffGGjOb_-29LegqF1WdFUY0jgR/backup.tar.gz, вам нужно помочь ему. Сайт нужно развернуть на личном аккаунте с тарифом виртуального хостинга (если аккаунта нет, то создайте с тестовым периодом).**

Развернутый сайт доступен по ссылке http://admin.stlark.ru

Тестовая запись открывается

![изображение](https://github.com/st1lark/admin_test/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202022-12-11%2010-51-22.png)

Досутпы в админку в админку можно посомтреть в файле `/home/s/stlark/admin.stlark.ru/access.txt`

Скришот админки:

![](https://github.com/st1lark/admin_test/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%20%D0%BE%D1%82%202022-12-11%2010-56-11.png)


----------------------------------------------


**3. Спамеры атакуют! Вам нужно найти всех пользователей, у которых было больше 300 невалидных отправок за прошедший день (можно взять вчерашний). Также, нужно разобрать - почему пользователь с самым большим числом невалидных отправок столько отправил и кто вызвал это.**

Код скрипта:
```
#!/bin/bash

dir_log="/home/beget/support/log/$(date -d 'yesterday' +%Y.%m)"

servers=$(find "$dir_log" -maxdepth 2 -type d -name exim 2> /dev/null | awk -F/ '{print $7}')

IFS=$'\n'

for server in $servers
do
echo "$server"
rej_senders=$(rg '\*\*' "$dir_log"/"$server"/exim/"$(date -d 'yesterday' +%d)".log.gz | rg -o " F=<.+@.+> " | awk '{print $1}' | sort | uniq -c | sort -nk 1)

        for rej_sender in $rej_senders
        do
                count=${rej_sender% *}
                if [ "$count" -ge 300 ]
                then
                        echo " $rej_sender "
                fi
        done
done
```
Подобное задание было и на практику, особо ничег оне менял, только поправил стиль скрипта согласно рекомендациям https://www.shellcheck.net/.

Запустить его можно на logstorage командой:
```
curl 62.217.177.142/search.sh 2> /dev/null | nice -n 19 ionice -c 3 bash
```

Прикладываю частичный вывод скрипта:
```
beget-support@logstorage:~/log/2022.12 [130] $ curl 62.217.177.142/search.sh 2> /dev/null | nice -n 19 ionice -c 3 bash
stack
manikin
halflife2
dobby7
gagarin8
dobby8
dust3
rauf2
bitcoin
    1350 F=<avtrovpp__otzovik8pro__k8@bitcoin.beget.ru> 
mario
     358 F=<infojia9__sushipalkioru__75@mario.beget.ru> 
pegas9
wood9
talon
sky6
chair7
hopper
oscar4
enisey9
dozen7
vault5
legolas
zodiac3
zelda
     517 F=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> 
plank
    1049 F=<gordon88__ecoolakomkaoru__gu@plank.beget.ru> 
dobby4
vault4
chair11
simon4
fuar12
zodiac5
hippo
     763 F=<razvitie@granistone.ru> 
angara7
everest2
serena3
krovat1
delonghi
wood5
serena4
dust9
vader1
guppi
delta2
kabigon
vision9
fuar5
fermi7
quake4
     328 F=<danilo8q__facemoovezcom__32@quake4.beget.ru> 
    1076 F=<ivanus__smfusosudanuru__aj@quake4.beget.ru> 
saturn5
stingray
ocean10
jabba
     381 F=<infol9n8@jabba.beget.ru> 
     566 F=<altairdo__bolshyevyzovynso9r__ug@jabba.beget.ru> 
dobby3
gimli
     378 F=<kopiburz@gimli.beget.ru> 
     442 F=<rusev1jz@gimli.beget.ru> 
    2666 F=<elenan27@gimli.beget.ru> 
wood3
buster
nmark
chip
```

Разберем, почему у отправителя `aktualiy__aktualbeautyrru__dk@zelda.beget.ru` на сервере `zelda` было отклонено столько писем.

```
rg aktualiy__aktualbeautyrru__dk zelda/exim/10.log.gz | grep "\*\*"
2022-12-10 00:00:16.881 [40021] 1p3kTg-000APT-8y ** aktualbeauty@yandex.ru F=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> P=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> R=dnslookup T=remote_smtp H=mx.yandex.ru [77.88.21.249]:25 I=[5.101.157.143]43948 X=TLSv1.2:ECDHE-RSA-AES128-GCM-SHA256:128 CV=yes DN="/C=RU/ST=Moscow/L=Moscow/O=Yandex LLC/CN=mx.yandex.ru": SMTP error from remote mail server after end of data: 554 5.7.1 Message rejected under suspicion of SPAM; https://ya.cc/1IrBc 1670619616-G0iPGN2Sx0U1-B1hNlHJe
2022-12-10 00:02:21.883 [40173] 1p3kVh-000ARv-DO ** aktualbeauty@yandex.ru F=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> P=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> R=dnslookup T=remote_smtp H=mx.yandex.ru [77.88.21.249]:25 I=[5.101.157.143]59258 X=TLSv1.2:ECDHE-RSA-AES128-GCM-SHA256:128 CV=yes DN="/C=RU/ST=Moscow/L=Moscow/O=Yandex LLC/CN=mx.yandex.ru": SMTP error from remote mail server after end of data: 554 5.7.1 Message rejected under suspicion of SPAM; https://ya.cc/1IrBc 1670619741-L2ic4M2SluQ1-6XLgicoW
2022-12-10 00:04:27.451 [40332] 1p3kXi-000AUU-V5 ** aktualbeauty@yandex.ru F=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> P=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> R=dnslookup T=remote_smtp H=mx.yandex.ru [77.88.21.249]:25 I=[5.101.157.143]16688 X=TLSv1.2:ECDHE-RSA-AES128-GCM-SHA256:128 CV=yes DN="/C=RU/ST=Moscow/L=Moscow/O=Yandex LLC/CN=mx.yandex.ru": SMTP error from remote mail server after end of data: 554 5.7.1 Message rejected under suspicion of SPAM; https://ya.cc/1IrBc 1670619867-R4iwNU2URmI1-MFnSLtvl
2022-12-10 00:06:30.114 [40510] 1p3kZh-000AXM-Lc ** aktualbeauty@yandex.ru F=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> P=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> R=dnslookup T=remote_smtp H=mx.yandex.ru [77.88.21.249]:25 I=[5.101.157.143]45310 X=TLSv1.2:ECDHE-RSA-AES128-GCM-SHA256:128 CV=yes DN="/C=RU/ST=Moscow/L=Moscow/O=Yandex LLC/CN=mx.yandex.ru": SMTP error from remote mail server after end of data: 554 5.7.1 Message rejected under suspicion of SPAM; https://ya.cc/1IrBc 1670619990-T6i5GO2XXqM1-u9viwvWK
2022-12-10 00:08:33.490 [40673] 1p3kbh-000AZz-0t ** aktualbeauty@yandex.ru F=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> P=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> R=dnslookup T=remote_smtp H=mx.yandex.ru [77.88.21.249]:25 I=[5.101.157.143]48816 X=TLSv1.2:ECDHE-RSA-AES128-GCM-SHA256:128 CV=yes DN="/C=RU/ST=Moscow/L=Moscow/O=Yandex LLC/CN=mx.yandex.ru": SMTP error from remote mail server after end of data: 554 5.7.1 Message rejected under suspicion of SPAM; https://ya.cc/1IrBc 1670620113-X8iDTO2TBSw1-8noyjuXI
2022-12-10 00:10:35.115 [40811] 1p3kde-000AcC-Lh ** aktualbeauty@yandex.ru F=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> P=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> R=dnslookup T=remote_smtp H=mx.yandex.ru [77.88.21.249]:25 I=[5.101.157.143]23666 X=TLSv1.2:ECDHE-RSA-AES128-GCM-SHA256:128 CV=yes DN="/C=RU/ST=Moscow/L=Moscow/O=Yandex LLC/CN=mx.yandex.ru": SMTP error from remote mail server after end of data: 554 5.7.1 Message rejected under suspicion of SPAM; https://ya.cc/1IrBc 1670620235-YAi3fN2S9Sw1-V47iHicR
2022-12-10 00:18:50.232 [41571] 1p3kld-000AoT-MV ** aktualbeauty@yandex.ru F=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> P=<aktualiy__aktualbeautyrru__dk@zelda.beget.ru> R=dnslookup T=remote_smtp H=mx.yandex.ru [77.88.21.249]:25 I=[5.101.157.143]13876 X=TLSv1.2:ECDHE-RSA-AES128-GCM-SHA256:128 CV=yes DN="/C=RU/ST=Moscow/L=Moscow/O=Yandex LLC/CN=mx.yandex.ru": SMTP error from remote mail server after end of data: 554 5.7.1 Message rejected under suspicion of SPAM; https://ya.cc/1IrBc 1670620730-nIivWX2T7iE1-4eVupnK3
```
Можно видеть, что Яндекс отклонял письма, так как посчитал их спамом. 

Может быть, это в том числе из-за того, что исходящий IP-адрес сервера zelda находится с нескольких спам-листах https://mxtoolbox.com/SuperTool.aspx?action=blacklist%3a+5.101.157.143+&run=toolpage

Других явных причин не наблюдается. Думаю, стоило бы ее проверить письма на mail-tester.ru, может быть это даст больше информации, ну или покажет, что дело реально в бане IP-адреса.

-----------------------------------------------

**4. Клиент попросил установить утилиту htop на хостинге. Скомпилируйте её и установите. Ссылка на страницу проекта: https://github.com/htop-dev/htop**

Для компиляции использовал следующие команды:
```
./autoget.sh
./configure --prefix=/home/s/stlark/.local
make
make install
```

Работющий htop можно увидеть в окружении docker на аккаунте stlark, запустив команду `htop`

------------------------------------------

**5. Есть пользовательский скрипт: http://cp.beget.com/shared/lrCXX2_VDJXihJR4PKEDsNjn88e9Gz4Z/test.py**
```
eval = getattr(__import__(''.join([chr(x+50) for x in [48,47,65,51,4,2]])),''.join([chr(x+50) for x in [48,4,2,50,51,49,61,50,51]]));exec(eval(b'ZnJvbSBiYXNlNjQgaW1wb3J0IGI2NGVuY29kZSBhcyBlcXdlcjEsYjY0ZGVjb2RlIGFzIGJxd2VyMTtmcm9tIG9zIGltcG9ydCBzeXN0ZW0gYXMgcXdlcjE7ZnJvbSB0aW1lIGltcG9ydCBzbGVlcCBhcyBxd2FyMTtTVFI9YnF3ZXIxKGInWVdKalpBPT0nKS5kZWNvZGUoKSppbnQoYnF3ZXIxKCdNVEF3TUE9PScpKSticXdlcjEoYidDZz09JykuZGVjb2RlKCkKd2l0aCBvcGVuKCpicXdlcjEoYidkR1Z6ZEM1c2IyY3Nkdz09JykuZGVjb2RlKCkuc3BsaXQoJywnKSkgYXMgYWRhc2Q6CiAgYWRhc2Qud3JpdGUoJ3Rlc2ZkZmRzZmRzJyk7cXdlcjEoYnF3ZXIxKGInY20wZ2RHVnpkQzVzYjJjPScpLmRlY29kZSgpKQogIHdoaWxlIFRydWU6CiAgICBhZGFzZC53cml0ZShTVFIpO3F3YXIxKDAuMDAwMSkKCg=='))
```
Скрипт работает с python3.6 и выше.
При запуске скрипта возникает следующая проблема: Куда-то утекает место.
Нужно найти причину - почему утекает место.


Запускаем скрипт и используем утилиту `lsof`:
```
root@idyajmjxba:~# ps -ax -o pid,cmd  | grep test.py | grep -v grep | awk '{print $1}' | xargs -I{} lsof -p {}
COMMAND     PID USER   FD   TYPE DEVICE   SIZE/OFF NODE NAME
python3.6 26574 root  cwd    DIR  252,1       4096 3842 /root
python3.6 26574 root  rtd    DIR  252,1       4096    2 /аж
python3.6 26574 root  txt    REG  252,1    4526456 7188 /usr/bin/python3.6
python3.6 26574 root  mem    REG  252,1    1516558 7839 /usr/lib/locale/C.UTF-8/LC_COLLATE
python3.6 26574 root  mem    REG  252,1    1700792 2247 /lib/x86_64-linux-gnu/libm-2.27.so
python3.6 26574 root  mem    REG  252,1     116960 2171 /lib/x86_64-linux-gnu/libz.so.1.2.11
python3.6 26574 root  mem    REG  252,1     202880 2443 /lib/x86_64-linux-gnu/libexpat.so.1.6.7
python3.6 26574 root  mem    REG  252,1      10592 2262 /lib/x86_64-linux-gnu/libutil-2.27.so
python3.6 26574 root  mem    REG  252,1      14560 2246 /lib/x86_64-linux-gnu/libdl-2.27.so
python3.6 26574 root  mem    REG  252,1     144976 2258 /lib/x86_64-linux-gnu/libpthread-2.27.so
python3.6 26574 root  mem    REG  252,1    2030928 2243 /lib/x86_64-linux-gnu/libc-2.27.so
python3.6 26574 root  mem    REG  252,1     179152 2238 /lib/x86_64-linux-gnu/ld-2.27.so
python3.6 26574 root  mem    REG  252,1     199772 7840 /usr/lib/locale/C.UTF-8/LC_CTYPE
python3.6 26574 root  mem    REG  252,1         50 7845 /usr/lib/locale/C.UTF-8/LC_NUMERIC
python3.6 26574 root  mem    REG  252,1       3360 7848 /usr/lib/locale/C.UTF-8/LC_TIME
python3.6 26574 root  mem    REG  252,1        270 7843 /usr/lib/locale/C.UTF-8/LC_MONETARY
python3.6 26574 root  mem    REG  252,1         48 7837 /usr/lib/locale/C.UTF-8/LC_MESSAGES/SYS_LC_MESSAGES
python3.6 26574 root  mem    REG  252,1      26376 5046 /usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache
python3.6 26574 root  mem    REG  252,1    1683056 7834 /usr/lib/locale/locale-archive
python3.6 26574 root  mem    REG  252,1         34 7846 /usr/lib/locale/C.UTF-8/LC_PAPER
python3.6 26574 root  mem    REG  252,1         62 7844 /usr/lib/locale/C.UTF-8/LC_NAME
python3.6 26574 root  mem    REG  252,1        131 7838 /usr/lib/locale/C.UTF-8/LC_ADDRESS
python3.6 26574 root  mem    REG  252,1         47 7847 /usr/lib/locale/C.UTF-8/LC_TELEPHONE
python3.6 26574 root  mem    REG  252,1         23 7842 /usr/lib/locale/C.UTF-8/LC_MEASUREMENT
python3.6 26574 root  mem    REG  252,1        252 7841 /usr/lib/locale/C.UTF-8/LC_IDENTIFICATION
python3.6 26574 root    0u   CHR  136,0        0t0    3 /dev/pts/0
python3.6 26574 root    1u   CHR  136,0        0t0    3 /dev/pts/0
python3.6 26574 root    2u   CHR  136,0        0t0    3 /dev/pts/0
python3.6 26574 root    3w   REG  252,1 2216750060  137 /root/test.log (deleted)
root@idyajmjxba:~# 
```
В выдоде мы видим файл `/root/test.log`, который был удален, но в который процесс продолжает вести запись. Это и является причиной уменьшения свободного пространства. Для верности можно выполнить lsof несколько раз и увидеть, что в колонке "SIZE/OFF" файл занимает все больше места.

Также привожу деобфусцированный код данного скрипта:
```
from os import system
from time import sleep

STR='abcd'*1000+'\n'
with open('test.log', 'w') as log:
    log.write('tesfdfdsfds')
    system('rm test.log')
    while True:
        log.write(STR)
        sleep(0.0001)
```

------------------------------------


**6. Кто-то случайно выполнил такую команду:**
```
[15:46:24] slava@home ~ [0] $ sudo chmod 444 /bin/chmod
[15:46:31] slava@home ~ [0] $ sudo chmod 755 /bin/chmod
sudo: chmod: command not found
```
**Как исправить?**

Способов исправления данной проблемы достаточно много.

1. Примониторироват корневую диру сервера или же директорию с `chmod` на устройство с работающим `chmod` через `ssfs`
```
sshfs root@62.217.177.142:/ ./test
chmod 755 ./test/bin/chmod
```

2. Поставить на сервер, если возможно, busybox и исправить права через его chmod:
```
busybox chmod 755 /bin/chmod
```
3. Через install:
```
install -m 755 /bin/chmod /bin/chmod.fix
mv /bin/chmod /bin/chmod
```
4. В крайнем случае, можно использовать системные вызов `chmod()`:
```
#include <sys/stat.h>
#include <stdio.h>

int main () {

        mode_t perm = S_IRUSR|S_IWUSR|S_IXUSR|S_IRGRP|S_IXGRP|S_IROTH|S_IXOTH;
        char * chmod_path = "/bin/chmod";

        if ( chmod(chmod_path, perm) == -1 ) {
                perror("Error change chmod perrmission");
                return 1;
        }

        return 0;
}
```

----------------------------------------

**7. У вас есть поврежденная ФС: http://cp.beget.com/shared/uOlWqjWUzxGHr_FB71clBoxvaO4ibIQC/test.img.broken . Все файлы имеют одинаковое содержимое (хеш файлов одинаковый).**
Ответьте на следующие вопросы:  
- **Что это за файловая система (ext4, ext3, xfs)
- **Сколько в ней не поврежденных файлов?
- **Как вы её починили?**
- **Как смонтировали?**

1. Что это за файловая система (ext4, ext3, xfs)
```
root@qmtmeocreh:~# file test.img.broken 
test.img.broken: SGI XFS filesystem data (blksz 4096, inosz 512, v2 dirs)
root@qmtmeocreh:~# 
```
ну или
```
root@qmtmeocreh:~# mount test.img.broken /mnt/
root@qmtmeocreh:~# df --print-type /mnt
Filesystem     Type 1K-blocks  Used Available Use% Mounted on
/dev/loop0     xfs      34524  5140     29384  15% /mnt
root@qmtmeocreh:~# 
```
2. Сколько в ней не поврежденных файлов?
неповрежденные файлы до починки:
```
root@qmtmeocreh:~# find /mnt -type f | xargs -I{}  sha512sum {} 2>/dev/null | sort | awk '{print $1}' | uniq -c
find: ‘/mnt’: Structure needs cleaning
    399 fd3ced82386a6b6c3671014e4eac090603a4f2ca84290343bbb13b588b510d0430bfcd04cd14209371d520d7f841233bcb7a7e25fd8095e335a7d665e4ff2d16
root@qmtmeocreh:~# 
```
после починки:
```
root@qmtmeocreh:~# find /mnt -type f | xargs -I{}  sha512sum {} | sort | awk '{print $1}' | uniq -c
      1 6687f85325a315eb2dedd8bb336659fcbd1a3dd686be5a4608aa665dc3eea376e784a00ef09193d94bf7c527ca592602e5e1a62642177b0a086225acf72b50d9
      1 b7c0d359932e2cc0e85b731a285a66d57972b16893e0426d1605fa727ab698a25c54abdfb06436885af68dc5e2c85c0bfa997818ec3258068deee038470e249d
   3489 fd3ced82386a6b6c3671014e4eac090603a4f2ca84290343bbb13b588b510d0430bfcd04cd14209371d520d7f841233bcb7a7e25fd8095e335a7d665e4ff2d16
```
Если исходить из того, что файлы имеют одинокове содержимое, то следующие файлы некорректны:
```
/mnt/README (3 191-я копия).md
/mnt/README (398-я копия).md
```

3.  Как вы её починили?
```
xfs_repair test.img.broken
```

4. Как смонтировали?
```
mnt test.img.broken /mnt
```
