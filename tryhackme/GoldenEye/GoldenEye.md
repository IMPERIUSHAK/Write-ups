# GoledenEye

So our first step is to connect to machine by installing opvn file and connect to it

`sudo openvpn name.opvn`---> to connect to machine

So after you connected to machine we should use nmap to look for open ports


As you can see in hint that we should use `Pn` so its bassicaly pings ports.
And also we should use `-p-` to ping all 65535 ports. it'll take some time

`nmap -Pn -p- <Target_IP>`

