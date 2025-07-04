To connect to Our target using RDP on windows

1. you simply type in search bar for `Remote Desktop Connection` or type `Win+R` than `mstsc` and type `<target_ip>` and credentials to connect
 
2. To get system info you should simply open cmd and type `systeminfo` ---> you'll see something like this Windows Server 2016

3. after you got connection you should to go `Event viewer`  to checkout all recent events.
and in event if you want to find out who log in  last time go to `windowslogs/security` there look for user who loged in before you.

Answer is:Adminisrator
4. When did John log onto the system last?

To find it out faster you should create Custom view and change `EventID` to `4624` this Event Id show success login information when `4625` is error to login.

And than `Find` for `John` last login.

5. What IP does the system connect to when it first starts?

Search for `System information.exe` than go to `Software Environment` and `Startup Programs`
there you can find `ip adress` and `command` when your System first starts.

Answer:10.34.2.3

6. What two accounts had administrative privileges (other than the Administrator user)?
 
Use this command to get users with Administrator privileges

7. Open Task Scheduler app 
click in `Task Scheduler Library` in the right side you can see name of tasks that has been Scheduled.
Open Task `clean file system` the open the actions there you can find the `command that is running` and `port` that is listening

Answer: Clean file system 

8. What file was the task trying to run daily?

Answer: nc.ps1

9. What port did this file listen locally for?
Answer: 1348

10. When did Jenny last logon?

So if we go search for Jenny as we look for John we could not find her it means that she never logedin.
You can get same result in terminal `net user Jenny`

Answer: never

11. At what date did the compromise take place? 

We can safely assume the date based on when the users were created.

Answer: 03/02/2019

12. During the compromise, at what time did Windows first assign special privileges to a new logon? Answer format: MM/DD/YYYY HH:MM:SS AM/PM

Open Event Viewer

Filter events on the day of the compromise.
In addition, filter events where a scheduled task might have elevated a privilege, when a user account was created, 
special privileges assigned to a new logon, and a logon was attempted using explicit credentials. 

Answer: 03/02/2019 4:04:49 PM

13. What tool was used to get Windows passwords?
I assume that malicious files could be in the “tmp” directory since we identified one before, where a program executed another program that establishes a network connection. 
I went to check that directory, and a suspicious-looking file is identified that starts with “mim”.

Answer:mimikatz

14. What was the attackers external control and command servers IP?

Let’s check the hosts file which is located at “C:\Windows\System32\drivers\etc”. 
This file serves as a local DNS allowing the local machine to map hostnames to IP addresses.

Looking at the file, definitely “google.com” does not use the IP address it was assigned to. 

Answer: 76.32.97.132

15. What was the extension name of the shell uploaded via the servers website?

Go to “C:\inetpub” – the location of web server log files for Microsoft Internet Information Services, 
which is the common web server for Windows. In it, there is another folder.

Answer: .jsp

16. Check for DNS poisoning, what site was targeted?

As you can see from hint that we should check firewall. Open firewall and click in `inbound rules`
in right side you should look for `Allow outside connection for development` click and open it to see more details
there you can find port that atacker opened

Answer:1377

17. Check for DNS poisoning, what site was targeted?

its definitely because default google.com dns is 8.8.8.8

Answer:google.com
