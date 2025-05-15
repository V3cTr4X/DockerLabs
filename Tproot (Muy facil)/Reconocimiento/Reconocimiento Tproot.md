```bash
┌─[root@a428b04153f3]─[/]
└──╼ #nmap 172.17.0.3
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-05-14 07:17 UTC
Nmap scan report for 172.17.0.3
Host is up (0.000012s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE
21/tcp open  ftp
80/tcp open  http
MAC Address: 02:42:AC:11:00:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds
┌─[root@a428b04153f3]─[/]
└──╼ #nmap -sS -sV -sC -p 21,80 172.17.0.3
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-05-14 07:17 UTC
Nmap scan report for 172.17.0.3
Host is up (0.000074s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
|_ftp-anon: got code 500 "OOPS: cannot change directory:/var/ftp".
80/tcp open  http    Apache httpd 2.4.58 ((Ubuntu))
|_http-server-header: Apache/2.4.58 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 02:42:AC:11:00:03 (Unknown)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.86 seconds
```

![[Pasted image 20250514092512.png]]

A primera vista tenemos un Apache sin nada y un FTP, vamos a ver que le puedo hacer, ya que es una version vulnerable

