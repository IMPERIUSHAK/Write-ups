How many open ports?

2. try nmap scan to get all open ports:

```
nmap -sC -sV 10.10.50.112

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c9:03:aa:aa:ea:a9:f1:f4:09:79:c0:47:41:16:f1:9b (RSA)
|   256 2e:1d:83:11:65:03:b4:78:e9:6d:94:d1:3b:db:f4:d6 (ECDSA)
|_  256 91:3d:e4:4f:ab:aa:e2:9e:44:af:d3:57:86:70:bc:39 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Beginning of the end
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```
3. What is the team name in operation?

If you check webpage you'll find somthing like this `STARS alpha team`


after some investigation i found new directory link in  `target_ip/mansionmain/` source after some reasearch in `/diningRoom/`
i found 2 more infomation

`SG93IGFib3V0IHRoZSAvdGVhUm9vbS8=` ---> in source 
`emblem{fec832623ea498e20bf4fe1821d58727}`---> in the link that was provided

I proceed to decode cipher:
```
echo "SG93IGFib3V0IHRoZSAvdGVhUm9vbS8="|base64 -d  
How about the /teaRoom/ ```
After decrypt i got new link (/teaRoom/).


4. What is the emblem flag?
 Answer: emblem{fec832623ea498e20bf4fe1821d58727}

5. What is the lock pick flag? 
So as expected i got some new link and flag in /teaRoom/ directory:
	lock_pick{037b35e2ff90916a9abf99129c8e1837}
	/artRoom/

Answer: lock_pick{037b35e2ff90916a9abf99129c8e1837}

6. What is the music sheet flag? 
 I found web map in `/artRoom/MansionMap/index.html` and after some research i found that in diningRoom apeared a 
input fot emblem flag so i used it, But nothing happens, So i continued my research.

After some research in rooms i used lockpick flag for BarRoom entrance, and found cipher:
	NV2XG2LDL5ZWQZLFOR5TGNRSMQ3TEZDFMFTDMNLGGVRGIYZWGNSGCZLDMU3GCMLGGY3TMZL5
```
echo "NV2XG2LDL5ZWQZLFOR5TGNRSMQ3TEZDFMFTDMNLGGVRGIYZWGNSGCZLDMU3GCMLGGY3TMZL5" | base32 -d
music_sheet{362d72deaf65f5bdc63daece6a1f676e}

```
Answer: `music_sheet{362d72deaf65f5bdc63daece6a1f676e}`

7. What is the gold emblem flag?

Yeap, I found music flag so I went back to barRoom and used. 
After using music flag i was redirecred to secret room i found new flag

So after getting golden emblem there was a message that i should refresh previous page. 
I did it, and found new input for emblem flag. After all i got a word `rebecca`--->i'll use it later

Answer: `gold_emblem{58a8c41a9d08b8a4e38d02a4d7ff4843}`

8. What is the shield key flag?
 After i reached /diningRoom2F/ there was a hidden maessage in source:

`Lbh trg gur oyhr trz ol chfuvat gur fgnghf gb gur ybjre sybbe. Gur trz vf ba gur qvavatEbbz svefg sybbe. Ivfvg fnccuver.ugzy`

so the type of this cipher is rot13 so i use [CyberCHief](https://gchq.github.io/CyberChef/#recipe=ROT13_Brute_Force(true,true,false,100,0,true,'')&input=TGJoIHRyZyBndXIgb3lociB0cnogb2wgY2hmdXZhdCBndXIgZmduZ2hmIGdiIGd1ciB5YmpyZSBzeWJiZS4gR3VyIHRyeiB2ZiBiYSBndXIgcXZhdmF0RWJieiBzdmVmZyBzeWJiZS4gSXZmdmcgZm5jY3V2ZXIudWd6eQ) to brutforce Rot13

this is the result ---> `You get the blue gem by pushing the status to the lower floor. The gem is on the diningRoom first fl`

now i should research /diningRoom/, so after some research i used golden emblem flag into input and get new cipher:

`klfvg ks r wimgnd biz mpuiui ulg fiemok tqod. Xii jvmc tbkg ks tempgf tyi_hvgct_jljinf_kvc`

it should be vigenere cipher and I already have key which is `rebecca` so after decoding in [CyberChief](https://gchq.github.io/CyberChef/#recipe=Vigen%C3%A8re_Decode('rebecca')&input=a2xmdmcga3MgciB3aW1nbmQgYml6IG1wdWl1aSB1bGcgZmllbW9rIHRxb2QuIFhpaSBqdm1jIHRia2cga3MgdGVtcGdmIHR5aV9odmdjdF9qbGppbmZfa3Zj)

I got this message:`there is a shield key inside the dining room. The html page is called the_great_shield_key`

So its the hidden page in dining room ---> the_great_shield_key.html, i decided to visit 
Yup, was with shield_key ---> `shield_key{48a7a9227cd7eb89f0a062590798cbac}`

9. What is blue gem flag?

I didn't find anything else in the previous directories, so I decided to go to a new one in the map we found (`/artRoom/MansionMap.html`).

I went to `/diningRoom2F/` and found another commented encoded string:

~~~
┌──(user㉿Y0B01)-[~/Desktop/walkthroughs/thm/Biohazard]
└─$ curl -s "http://$IP/diningRoom2F/" | grep "<\!--"
	<!-- Lbh trg gur oyhr trz ol chfuvat gur fgnghf gb gur ybjre sybbe. Gur trz vf ba gur qvavatEbbz svefg sybbe. Ivfvg fnccuver.ugzy -->
~~~

We are looking at a ROT13 cipher. You can find several online tools to decode it. It decodes to:

```
You get the blue gem by pushing the status to the lower floor. The gem is on the diningRoom first floor. Visit sapphire.html
```

Let's do it then. Navigate to `/diningRoom/sapphire.html` and we have the blue gem flag now:

~~~
┌──(user㉿Y0B01)-[~/Desktop/walkthroughs/thm/Biohazard]
└─$ curl -s "http://$IP/diningRoom/sapphire.html" 
blue_jewel{e1d457e96cac640f863ec7bc475d48aa}
~~~

Blue gem flag: `blue_jewel{e1d457e96cac640f863ec7bc475d48aa}`

10. What is the FTP username?
I assume that to get ftp username name and password only if i all crest from 1-4, so lets do it

The first crest is in /tigerStatusRoom/ i past jewel flag in input, to get first crest

```
crest 1:
S0pXRkVVS0pKQkxIVVdTWUpFM0VTUlk9
Hint 1: Crest 1 has been encoded twice
Hint 2: Crest 1 contanis 14 letters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
```
 Base64->Base32:
` RlRQIHVzZXI6IG`

```

crest 2

GVFWK5KHK5WTGTCILE4DKY3DNN4GQQRTM5AVCTKE
Hint 1: Crest 2 has been encoded twice
Hint 2: Crest 2 contanis 18 letters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
```
 Base32->Base58->`h1bnRlciwgRlRQIHBh`

```

crest 3

MDAxMTAxMTAgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMDAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMTEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMDEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTEwMDA=
Hint 1: Crest 3 has been encoded three times
Hint 2: Crest 3 contanis 19 letters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
```
 Base64->Binary->Hex->`c3M6IHlvdV9jYW50X2h`

```

crest 4

gSUERauVpvKzRpyPpuYz66JDmRTbJubaoArM6CAQsnVwte6zF9J4GGYyun3k5qM9ma4s
Hint 1: Crest 2 has been encoded twice
Hint 2: Crest 2 contanis 17 characters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
```
Base58->hex->`pZGVfZm9yZXZlcg==`


Now that I decoded all parts, I proceed to the next phrase(combine all, and decode):

`RlRQIHVzZXI6IGh1bnRlciwgRlRQIHBhc3M6IHlvdV9jYW50X2hpZGVfZm9yZXZlcg==`

```
─$ echo "RlRQIHVzZXI6IGh1bnRlciwgRlRQIHBhc3M6IHlvdV9jYW50X2hpZGVfZm9yZXZlcg==" | base64 -d
FTP user: hunter, FTP pass: you_cant_hide_forever  
```
Answer: FTP user ->hunter, FTP->pass you_cant_hide_forever


To get more information i connected to ftp:

After connecting to ftp i found another cipher which contains of three parts, i donwloaded them started decipher

```
ftp 10.10.178.43 
Connected to 10.10.178.43.
220 (vsFTPd 3.0.3)
Name (10.10.178.43:guts): hunter
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -al
229 Entering Extended Passive Mode (|||15405|)
150 Here comes the directory listing.
drwxrwxrwx    2 1002     1002         4096 Sep 20  2019 .
drwxrwxrwx    2 1002     1002         4096 Sep 20  2019 ..
-rw-r--r--    1 0        0            7994 Sep 19  2019 001-key.jpg
-rw-r--r--    1 0        0            2210 Sep 19  2019 002-key.jpg
-rw-r--r--    1 0        0            2146 Sep 19  2019 003-key.jpg
-rw-r--r--    1 0        0             121 Sep 19  2019 helmet_key.txt.gpg
-rw-r--r--    1 0        0             170 Sep 20  2019 important.txt
226 Directory send OK.
ftp> get * 
local: 001-key.jpg remote: *
229 Entering Extended Passive Mode (|||39364|)
550 Failed to open file.
ftp> mget *
mget 001-key.jpg [anpqy?]? anpqy
Prompting off for duration of mget.
229 Entering Extended Passive Mode (|||31971|)
150 Opening BINARY mode data connection for 001-key.jpg (7994 bytes).
100% |***********************************************************************|  7994       91.35 KiB/s    00:00 ETA
226 Transfer complete.
7994 bytes received in 00:00 (28.33 KiB/s)
229 Entering Extended Passive Mode (|||11101|)
150 Opening BINARY mode data connection for 002-key.jpg (2210 bytes).
100% |***********************************************************************|  2210      237.71 KiB/s    00:00 ETA
226 Transfer complete.
2210 bytes received in 00:00 (13.94 KiB/s)
229 Entering Extended Passive Mode (|||35567|)
150 Opening BINARY mode data connection for 003-key.jpg (2146 bytes).
100% |***********************************************************************|  2146        2.67 KiB/s    00:00 ETA
226 Transfer complete.
2146 bytes received in 00:00 (2.30 KiB/s)
229 Entering Extended Passive Mode (|||30353|)
150 Opening BINARY mode data connection for helmet_key.txt.gpg (121 bytes).
100% |***********************************************************************|   121        1.88 KiB/s    00:00 ETA
226 Transfer complete.
121 bytes received in 00:00 (0.45 KiB/s)
229 Entering Extended Passive Mode (|||30655|)
150 Opening BINARY mode data connection for important.txt (170 bytes).
100% |***********************************************************************|   170       44.99 KiB/s    00:00 ETA
226 Transfer complete.
170 bytes received in 00:00 (0.64 KiB/s)
```

001-key.jpg

```
─$ steghide extract -sf 001-key.jpg           
Enter passphrase: 
wrote extracted data to "key-001.txt".
                                                                                                                                                                                                                                            
┌──(guts㉿TORFF)-[~]
└─$ cat key-001.txt  
cGxhbnQ0Ml9jYW

```

Answer: **cGxhbnQ0Ml9jYW**

002-key.jpg

```
┌──(guts㉿TORFF)-[~]
└─$ exiftool 002-key.jpg 
ExifTool Version Number         : 13.25
File Name                       : 002-key.jpg
Directory                       : .
File Size                       : 2.2 kB
File Modification Date/Time     : 2019:09:19 01:08:31-05:00
File Access Date/Time           : 2025:07:17 02:43:06-05:00
File Inode Change Date/Time     : 2025:07:17 02:43:06-05:00
File Permissions                : -rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Comment                         : 5fYmVfZGVzdHJveV9
Image Width                     : 100
Image Height                    : 80
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 100x80
Megapixels                      : 0.008
                                         
```

Answer: **5fYmVfZGVzdHJveV9**

003-key.jpg

```
$ binwalk 003-key.jpg -e

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
1930          0x78A           Zip archive data, at least v2.0 to extract, uncompressed size: 14, name: key-003.txt

WARNING: One or more files failed to extract: either no utility was found or it's unimplemented


(guts㉿TORFF)-[~]
└─$ cat _003-key.jpg.extracted/key-003.txt 
3aXRoX3Zqb2x0

```

Answer:**3aXRoX3Zqb2x0**


So the final result is:
`cGxhbnQ0Ml9jYW5fYmVfZGVzdHJveV93aXRoX3Zqb2x0`

```
└─$ echo "cGxhbnQ0Ml9jYW5fYmVfZGVzdHJveV93aXRoX3Zqb2x0" | base64 -d
plant42_can_be_destroy_with_vjolt  
```
11. Password for the encrypted file?

Answer: **plant42_can_be_destroy_with_vjolt **

So it should be the key for `helmet_key.txt.gpg` encrypted file

```
└─$ gpg helmet_key.txt.gpg
gpg: WARNING: no command supplied.  Trying to guess what you mean ...
gpg: AES256.CFB encrypted data
gpg: encrypted with 1 passphrase

└─$ cat helmet_key.txt                    
helmet_key{458493193501d2b94bbab2e727f8db4b}
```
 As expected it was a new flag: `helmet_key{458493193501d2b94bbab2e727f8db4b}`

12. What is the helmet key flag?

Answer: **helmet_key{458493193501d2b94bbab2e727f8db4b} **

13. Where is the hidden directory mentioned by Barry?

Answer: **/hidden_closet/ **


After i got a helmet flag, i return to /studyRoom/ to Submit it, After submition i got tar.gz file `doom.tar.gz`

``` 
tar -xvf doom.tar.gz
eagle_medal.txt
 cat eagle_medal.txt 
SSH user: umbrella_guest

```
What is the SSH login username?

Answer: **umbrella_guest**

What is the SSH login password?

I found password name of the leader,and encrypted message in hiden directory `hidden_closet`


What is the SSH login password?
**`T_virus_rules`**

Who the STARS bravo team leader

**`Enrico`**



## Task 5

After connecting to ssh i found something interesting:
```

umbrella_guest@umbrella_corp:~$ ls -la
total 64
drwxr-xr-x  8 umbrella_guest umbrella 4096 Sep 20  2019 .
drwxr-xr-x  5 root           root     4096 Sep 20  2019 ..
-rw-r--r--  1 umbrella_guest umbrella  220 Sep 19  2019 .bash_logout
-rw-r--r--  1 umbrella_guest umbrella 3771 Sep 19  2019 .bashrc
drwxrwxr-x  6 umbrella_guest umbrella 4096 Sep 20  2019 .cache
drwxr-xr-x 11 umbrella_guest umbrella 4096 Sep 19  2019 .config
-rw-r--r--  1 umbrella_guest umbrella   26 Sep 19  2019 .dmrc
drwx------  3 umbrella_guest umbrella 4096 Sep 19  2019 .gnupg
-rw-------  1 umbrella_guest umbrella  346 Sep 19  2019 .ICEauthority
drwxr-xr-x  2 umbrella_guest umbrella 4096 Sep 20  2019 .jailcell
drwxr-xr-x  3 umbrella_guest umbrella 4096 Sep 19  2019 .local
-rw-r--r--  1 umbrella_guest umbrella  807 Sep 19  2019 .profile
drwx------  2 umbrella_guest umbrella 4096 Sep 20  2019 .ssh
-rw-------  1 umbrella_guest umbrella  109 Sep 19  2019 .Xauthority
-rw-------  1 umbrella_guest umbrella 7546 Sep 19  2019 .xsession-errors
umbrella_guest@umbrella_corp:~$ ls .jailcell/
chris.txt                                                                                                           
umbrella_guest@umbrella_corp:~$ cat .jailcell/chris.txt 
Jill: Chris, is that you?                                                                                           
Chris: Jill, you finally come. I was locked in the Jail cell for a while. It seem that weasker is behind all this.  
Jil, What? Weasker? He is the traitor?                                                                              
Chris: Yes, Jill. Unfortunately, he play us like a damn fiddle.                                                     
Jill: Let's get out of here first, I have contact brad for helicopter support.                                      
Chris: Thanks Jill, here, take this MO Disk 2 with you. It look like the key to decipher something.                 
Jill: Alright, I will deal with him later.                                                                          
Chris: see ya.                                                                                                      
                                                                                                                    
MO disk 2: albert                                                                                                   
umbrella_guest@umbrella_corp:~$    
```

Here is the answer to 3/5 question in 5 Task

Where you found Chris?

Answer: **jailcell**

Who is the traitor?

Answer: **weasker**

I noticed that `MO disk 2: albert` is key for cipher(Vigenere), which was in /hiddencloset/. And it gives weasker ssh password.

The login password for the traitor?

Answer: **stars_members_are_my_guinea_pig**


So i conected as weasker and got all information i need to finish this task

```

weasker@umbrella_corp:~$ ls -la
total 80
drwxr-xr-x  9 weasker weasker 4096 Sep 20  2019 .
drwxr-xr-x  5 root    root    4096 Sep 20  2019 ..
-rw-------  1 weasker weasker   18 Sep 20  2019 .bash_history
-rw-r--r--  1 weasker weasker  220 Sep 18  2019 .bash_logout
-rw-r--r--  1 weasker weasker 3771 Sep 18  2019 .bashrc
drwxrwxr-x 10 weasker weasker 4096 Jul 17 02:34 .cache
drwxr-xr-x 11 weasker weasker 4096 Sep 20  2019 .config
drwxr-xr-x  2 weasker weasker 4096 Sep 19  2019 Desktop
drwx------  3 weasker weasker 4096 Sep 19  2019 .gnupg
-rw-------  1 weasker weasker  346 Sep 20  2019 .ICEauthority
drwxr-xr-x  3 weasker weasker 4096 Sep 19  2019 .local
drwx------  5 weasker weasker 4096 Sep 19  2019 .mozilla
-rw-r--r--  1 weasker weasker  807 Sep 18  2019 .profile
drwx------  2 weasker weasker 4096 Sep 19  2019 .ssh
-rw-r--r--  1 weasker weasker    0 Sep 20  2019 .sudo_as_admin_successful
-rw-r--r--  1 root    root     534 Sep 20  2019 weasker_note.txt
-rw-------  1 weasker weasker  109 Sep 20  2019 .Xauthority
-rw-------  1 weasker weasker 5548 Sep 20  2019 .xsession-errors
-rw-------  1 weasker weasker 6749 Sep 20  2019 .xsession-errors.old
weasker@umbrella_corp:~$ sudo -l
[sudo] password for weasker: 
Matching Defaults entries for weasker on umbrella_corp:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User weasker may run the following commands on umbrella_corp:
    (ALL : ALL) ALL
weasker@umbrella_corp:~$ cat weasker_note.txt 
Weaker: Finally, you are here, Jill.
Jill: Weasker! stop it, You are destroying the  mankind.
Weasker: Destroying the mankind? How about creating a 'new' mankind. A world, only the strong can survive.
Jill: This is insane.
Weasker: Let me show you the ultimate lifeform, the Tyrant.

(Tyrant jump out and kill Weasker instantly)
(Jill able to stun the tyrant will a few powerful magnum round)

Alarm: Warning! warning! Self-detruct sequence has been activated. All personal, please evacuate immediately. (Repeat)
Jill: Poor bastard


weasker@umbrella_corp:~$ ls /root
ls: cannot open directory '/root': Permission denied
weasker@umbrella_corp:~$ sudo ls /root
root.txt
weasker@umbrella_corp:~$ sudo cat /root/root.txt
In the state of emergency, Jill, Barry and Chris are reaching the helipad and awaiting for the helicopter support.

Suddenly, the Tyrant jump out from nowhere. After a tough fight, brad, throw a rocket launcher on the helipad. Without thinking twice, Jill pick up the launcher and fire at the Tyrant.

The Tyrant shredded into pieces and the Mansion was blowed. The survivor able to escape with the helicopter and prepare for their next fight.

The End

flag: 3c5794a00dc56c35f2bf096571edf3bf
weasker@umbrella_corp:~$ 
```


The name of the ultimate form?

Answer: **Tyrant**

The root flag?

Answer: **3c5794a00dc56c35f2bf096571edf3bf**
