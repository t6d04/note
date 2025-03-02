---
title: THM - Chesse CTF
date: 02-03-2025
category: ctf
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
Để exploit được lab này, ta cần học về PHP Filter Chain.
