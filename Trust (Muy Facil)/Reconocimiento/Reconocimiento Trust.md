
``
```bash
  ❯❯ /home/v3ctr4x/Escritorio/DockerLabs : sudo nmap -p 22,80 -sS -sV -sC 172.19.0.2
[sudo] contraseña para v3ctr4x: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-06 12:12 CEST
Nmap scan report for 172.19.0.2
Host is up (0.0090s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 19:a1:1a:42:fa:3a:9d:9a:0f:ea:91:7f:7e:db:a3:c7 (ECDSA)
|_  256 a6:fd:cf:45:a6:95:05:2c:58:10:73:8d:39:57:2b:ff (ED25519)
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.57 (Debian)
MAC Address: 5E:9F:EC:F6:C3:5F (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.05 seconds

  ❯❯ /home/v3ctr4x/Escritorio/DockerLabs : nmap 172.19.0.2
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-06 12:12 CEST
Nmap scan report for 172.19.0.2
Host is up (0.016s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 3.90 seconds

```

![[Pasted image 20250506121530.png]]

