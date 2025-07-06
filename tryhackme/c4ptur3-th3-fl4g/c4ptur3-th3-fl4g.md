

1. c4n y0u c4p7u23 7h3 f149?
 So to this question you should answer just by reading it in corect way

Answer: can you capture the flag?

2. 01101100 01100101 01110100 01110011 00100000 01110100 01110010 01111001 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00100000 01101111 01110101 01110100 00100001

To find out what is here we can go and write some script to solve it here is how:

First we should delete space between them and write it to the `file.bin` 

```
tr -d ' '<try.bin>file.bin #try.bin is encoded txt from that we deleting spaces and saving it in file.bin

```
after we did that we can write our own script to decode or just google for bin to dec decoder web site it'll be faster.
But if you want to know to write your own script to decode, here is the code:

```
#!/bin/bash

bin_to_char() {
        local bin = $1
        echo $((2#$bin)) | awk '{printf "%c", $1}'
}

read -r line < "path/to/file.bin"


for ((i=0; i<$(#line); i+=8)); do
        byte=${line:i:8}

        if [ ${#byte} -lt 8 ]; then
                while [${#byte} -lt 8]; do
                        byte += "0"
                done
        fi

        bin_to_char "$byte"
done

echo
```
and you simply should run it
```
sudo chmod 700 <script_name>.sh


./<script_name>.sh

lets try some binary out!

```


3. MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======

`echo MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM====== | base32 -d`


`base32 is super common in CTF's`

4. RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg==

`echo RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg== | base64 -d`

`Each Base64 digit represents exactly 6 bits of data.`

5. 68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f

```
echo "68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f" | tr -d ' '

68657861646563696d616c206f72206261736531363f


echo "68657861646563696d616c206f72206261736531363f" | xxd -r -p

hexadecimal or base16?
```
6. Ebgngr zr 13 cynprf! you can find answer to this in https://dencode.com/

`Rotate me 13 places!`

7. *@F DA:? >6 C:89E C@F?5 323J C:89E C@F?5 Wcf E:>6DX 

Answer:You spin me right round baby right round (47 times)

8.  - . .-.. . -.-. --- -- -- ..- -. .. -.-. .- - .. --- -.

. -. -.-. --- -.. .. -. --. 

So this is Morse alphabet to decode it you can look for decoder in google 

`telecommunication encoding`

9. `85 110 112 97 99 107 32 116 104 105 115 32 66 67 68` to this is just an ascii decimal,  all you have to do is translate to characters

`echo "85 110 112 97 99 107 32 116 104 105 115 32 66 67 68" | awk '{ for (i=1; i<=NF; i++) printf "%c", $i }'`

Unpack this BCD

10. Its just a combination of previous cyphers

    *Cipher 1: Base64
    *Cipher 2: Morse code
    *Cipher 3: Binary to ASCII
    *Cipher 4: ROT47
    *Cipher 5: BCD to ASCII

Answer:`Let's make this a bit trickier...`
#Task 2

Spectogram ---> in this task you should install `audicity` to what actually playing this audio 

after install drop audio to the app to the program

change waveform to spectogram to see the answer

https://github.com/pamhrituc/TryHackMe_Writeups/blob/master/room_c4ptur3_th3_fl4g/screenshots/audacity.png?raw=true

Answer: `Super Secret Message`

#Task 3

Steganography is the practice of concealing a file, message, image, or video within another file, message, image, or video.

Here to get secret message from image we should use `steghide`

`steghide extract -sf downloaded.jpg`

then go to file file that was created after that command


`SpaghettiSteg`

#Task 4 

we cant get access to steghide because of passphrase, so here we should use `strings`.

strings - print the sequences of printable characters in files

`strings image.jpg`

Answer hackerchat.png, AHH_YOU_FOUND_ME!
