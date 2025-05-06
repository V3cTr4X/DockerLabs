```bash
┌──(root㉿112c1bb279f3)-[/]
└─# nmap 172.17.0.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-05 07:37 UTC
Nmap scan report for 172.17.0.3
Host is up (0.0000030s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 02:42:AC:11:00:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 13.12 seconds
```

```bash
└─# nmap -sS -sC -sV -p 22 172.17.0.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-05 07:39 UTC
Nmap scan report for 172.17.0.3
Host is up (0.000046s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.7 (protocol 2.0)
| ssh-hostkey: 
|   2048 1a:cb:5e:a3:3d:d1:da:c0:ed:2a:61:7f:73:79:46:ce (RSA)
|   256 54:9e:53:23:57:fc:60:1e:c0:41:cb:f3:85:32:01:fc (ECDSA)
|_  256 4b:15:7e:7b:b3:07:54:3d:74:ad:e0:94:78:0c:94:93 (ED25519)
MAC Address: 02:42:AC:11:00:03 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.21 seconds
```

