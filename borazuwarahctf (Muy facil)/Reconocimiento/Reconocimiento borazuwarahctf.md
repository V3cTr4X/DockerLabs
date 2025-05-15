```bash
┌──(root㉿38095a406ad0)-[~]
└─# nmap 172.17.0.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-14 09:47 UTC
Nmap scan report for 172.17.0.3
Host is up (0.000011s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
MAC Address: 02:42:AC:11:00:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.23 seconds

┌──(root㉿38095a406ad0)-[~]
└─# nmap --open -sC -sV -sS -p 22,80 172.17.0.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-14 09:48 UTC
Nmap scan report for 172.17.0.3
Host is up (0.000079s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 3d:fd:d7:c8:17:97:f5:12:b1:f5:11:7d:af:88:06:fe (ECDSA)
|_  256 43:b3:ba:a9:32:c9:01:43:ee:62:d0:11:12:1d:5d:17 (ED25519)
80/tcp open  http    Apache httpd 2.4.59 ((Debian))
|_http-title: Site doesnt have a title (text/html).
|_http-server-header: Apache/2.4.59 (Debian)
MAC Address: 02:42:AC:11:00:03 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.94 seconds

```