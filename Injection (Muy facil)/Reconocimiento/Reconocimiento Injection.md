```bash
  ❯❯ /home/v3ctr4x : nmap 172.17.0.2
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-30 10:28 CEST
Nmap scan report for 172.17.0.2
Host is up (0.036s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 2.18 seconds
```

```bash
  ❯❯ /home/v3ctr4x : sudo nmap -p 22,80 -sS -sV -sC 172.17.0.2
[sudo] contraseña para v3ctr4x: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-30 10:29 CEST
Stats: 0:00:07 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 98.95% done; ETC: 10:29 (0:00:00 remaining)
Nmap scan report for 172.17.0.2
Host is up (0.0091s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 72:1f:e1:92:70:3f:21:a2:0a:c6:a6:0e:b8:a2:aa:d5 (ECDSA)
|_  256 8f:3a:cd:fc:03:26:ad:49:4a:6c:a1:89:39:f9:7c:22 (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Iniciar Sesi\xC3\xB3n
|_http-server-header: Apache/2.4.52 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
MAC Address: B2:FF:87:29:E9:3E (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.27 seconds
```

![[Pasted image 20250430103058.png]]

Tenemos un Login, por el nombre de la maquina me espero cualquier cosa, probare con una injeccion SQL, la hare con sqlmap, pero antes le pasare un Gobuster para ver que nos podemos encontrar dentro del directorio de la web