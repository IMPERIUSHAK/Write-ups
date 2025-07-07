First thing we should do is to run `nmap` for open ports:

`nmap -A -v <target_ip>`

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 e8:da:b7:0d:a7:a1:cc:8e:ac:4b:19:6d:25:2b:3e:77 (RSA)
|   256 c1:0c:5a:db:6c:d6:a3:15:96:85:21:e9:48:65:28:42 (ECDSA)
|_  256 0f:1a:6a:d1:bb:cb:a6:3e:bd:8f:99:8d:da:2f:30:86 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Default Page
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/StuxCTF/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS

```
after nmap scan we get 2 open ports lets check port 80.

http//:<target_ip>:80

after you get into website check view-source you would see this encoded text:

```

	<!-- The secret directory is...
		p: 9975298661930085086019708402870402191114171745913160469454315876556947370642799226714405016920875594030192024506376929926694545081888689821796050434591251;
		g: 7;
		a: 330;
		b: 450;
		g^c: 6091917800833598741530924081762225477418277010142022622731688158297759621329407070985497917078988781448889947074350694220209769840915705739528359582454617;
		-->


```
It's Diffie-hellman key exchange.To get key the hidden directory we should solve it, this is how i did it with bash script:

```
$ nano Duffle-hellman.sh 
#!/bin/bash

P=$1
a=$2
b=$3
gc=$4

gac=$(echo "$gc^$a % $P" | bc)
gabc=$(echo "$gac^$b % $P" | bc)

echo $gabc

$chmod 700 Duffle-hellman.sh

$./Duffle-hellman.sh

```
After running the script, you may get the following error:
./Duffle-hellman.sh: line 9: bc: command not found
This means that the bc package is not installed on your system.

Basically, Bash scripts can't handle large numbers directly, so bc is required to perform operations with big integers.

In this case, bc outputs very large numbers, and when Bash prints them, it splits long lines using the backslash (\) symbol.

If you run the script again, you'll get an output like this:

``` 
47315028937264895539131328176684350732577039984023005189203993885687\
32895380420270497705080780083292819852656706944604442285505527079932\
762337253841976839
```
To get the final result, remove the backslashes (\) and join the lines into one. Then take the first 128 characters — that will be the answer to question 3.

Answer:
47315028937264895539131328176684350732577039984023005189203993885687328953804202704977050807800832928198526567069446044422855055

