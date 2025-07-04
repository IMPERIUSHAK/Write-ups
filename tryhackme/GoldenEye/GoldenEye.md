# GoledenEye

So our first step is to connect to machine by installing opvn file and connect to it

`sudo openvpn name.opvn`---> to connect to machine

So after you connected to machine we should use nmap to look for open ports


As you can see in hint that we should use `Pn` so its bassicaly pings ports.
And also we should use `-p-` to ping all 65535 ports. it'll take some time

`nmap -Pn -p- <Target_IP>`---> for me it takes so long to fully scan the network (6-7hours)

and now you can answer to question 

Use nmap to scan the network for all ports. How many ports are open? --> 4

Also you can see that we have open port `80` which is `http` go and check it out

`http://<target_IP>` ---> check view source of this site, you can find some answers like:

Who needs to make sure they update their default password?--->boris

and if go to `http://<target_IP>/terminal.js` you'll find hex encoded password to decode it just simpy past in browser it'll automaticly decode it because of Percent-encoding and you'll get boris old password InvincibleHack3r



