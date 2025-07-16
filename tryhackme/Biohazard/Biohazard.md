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

9. 

