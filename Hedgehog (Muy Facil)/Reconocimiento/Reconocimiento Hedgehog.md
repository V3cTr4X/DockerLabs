```bash
┌──(root㉿38095a406ad0)-[/]
└─# nmap 172.17.0.2
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-15 06:56 UTC
Nmap scan report for 172.17.0.2
Host is up (0.000014s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
MAC Address: 02:42:AC:11:00:02 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.32 seconds

┌──(root㉿38095a406ad0)-[/]
└─# nmap --open -sC -sV -sS -p 22,80 172.17.0.2
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-15 06:56 UTC
Nmap scan report for 172.17.0.2
Host is up (0.000092s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 34:0d:04:25:20:b6:e5:fc:c9:0d:cb:c9:6c:ef:bb:a0 (ECDSA)
|_  256 05:56:e3:50:e8:f4:35:96:fe:6b:94:c9:da:e9:47:1f (ED25519)
80/tcp open  http    Apache httpd 2.4.58 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.58 (Ubuntu)
MAC Address: 02:42:AC:11:00:02 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.94 seconds


```