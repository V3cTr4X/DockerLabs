```bash
┌──(root㉿112c1bb279f3)-[/]
└─# nmap 172.17.0.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-06 07:21 UTC
Stats: 0:00:03 elapsed; 0 hosts completed (0 up), 1 undergoing ARP Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Stats: 0:00:04 elapsed; 0 hosts completed (0 up), 1 undergoing ARP Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Nmap scan report for 172.17.0.3
Host is up (0.000010s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
21/tcp open  ftp
MAC Address: 02:42:AC:11:00:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 13.12 seconds

┌──(root㉿112c1bb279f3)-[/]
└─# nmap -sS -sC -sV -T 5 -p 21 172.17.0.3
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-06 07:22 UTC
Nmap scan report for 172.17.0.3
Host is up (0.000042s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
MAC Address: 02:42:AC:11:00:03 (Unknown)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.46 seconds
```

Tenemos unicamente un puerto disponible dentro de la maquina, vamos a tener que analizarlo, para ver que podemos sacar de el