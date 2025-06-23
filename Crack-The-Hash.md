#from 1.1-1.3 you will work with same script

1.First of all to crack the hash you should know with wich type of hash you are working and this utils and web-site will help you.
hashid, hash-identifier, [hashes](https://hashes.com/en/tools/hash_identifier)

2.Second step is find out how to work with hashcat, so bassicly you read manual or watch tutorials

Example:
$hashcat -m 0 -a 0 48bb6e862e54f2a795ffc4e541caed4d  /path/to/wordlist 

# 1.4
We have the following hash:
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom

Hint: A straight brute-force attack would take too long, and the password contains only 4 characters.
1. Identify the hash type

recomend:[hashes](https://hashes.com/en/tools/hash_identifier)

The result will look something like this:```
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom - Possible algorithms: bcrypt $2*$, Blowfish (Unix)```
2. Create a regex to extract only 4-character words from the wordlist
bash

grep -x -E '[a-z]{4}' /usr/share/wordlists/rockyou.txt > outrock.txt

3. Crack the hash using the filtered wordlist
bash

echo '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom' > hash.txt  
hashcat -m 3200 -a 0 hash.txt outrock.txt
# 1.5

#1.5

We have this hash:
`279412f945939ba78ce0758d3fd83daa`

The hash type is MD5, so it should be easy to crack, right?
However, when trying to crack it with just rockyou.txt, we get no results.

The next step is to:

    Try a different wordlist, or

    Use base64.rule to encode guessed passwords in Base64 (in case the original password was Base64-encoded).

`hashcat -m 900 -a 0 outhash.txt /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule`
