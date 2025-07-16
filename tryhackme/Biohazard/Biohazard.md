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

10. 
