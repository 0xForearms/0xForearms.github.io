Name:   Artic  
OS:     Windows  
IP:     10.10.10.111  

To start off on Artic I ran my normal Nmap scan and found ports 135, 8500, and 49154 open.

    sudo nmap -Pn -vv -sV -oN nmap/initial 10.10.10.11


![ArticNmap.png](./resources/artic/ArticNmap.png)

Looking at ports 135 and 49154 I found no leads, however port 8500 appeared to be a CMS site, specifically for Adobe Coldfusion 8.  

![ArticSite.png](./resources/artic/ArticSite.png)


Looking for some premade exploits via Searchsploit leads us to quite a few options. 

![ArticSS1.png](./resources/artic/ArticSS1.png)


After going through a few of the exploits, we land on a Directory Traversal exploit.  

![ArticSS2.png](./resources/artic/ArticSS2.png)

Running this exploit  gives us something promising.

![ArticDT.png](./resources/artic/ArticDT.png)

Based off the 'encrypted=true' I'm going to assume it's hashed in some way.  So lets pass this over to John and see if we can crack it.   

![ArticJohn.png](./resources/artic/ArticJohn.png)

Success!  Looks like we have a password now.  Based off my notes, I don't believe we know the username at this point, so lets try some default ones, starting with 'Administrator'

![ArticHomePage.png](./resources/artic/ArticHomePage.png)

We're in!  
Looking around for methods of getting a shell, I stubmled upon this [youtube video](https://www.youtube.com/watch?v=NRMhZ3tofpM) showing me how.    

Following along, I was able to get a shell.  
![ArticHttp1.png](./resources/artic/ArticHttp1.png)
![ArticShell.png](./resources/artic/ArticShell.png)

When I did this, I struggled with Windows boxes some, so I pivoted to the MSF route.  I used Powershell to download a meterpreter embeded file and upgraded my shell.  This was before I knew about the Shell_to_Meterpreter Module within Metasploit.

![ArticMeterpreter1.png](./resources/artic/ArticMeterpreter1.png)
![ArticMeterpreter2.png](./resources/artic/ArticMeterpreter2.png)
![ArticMeterpreter3.png](./resources/artic/ArticMeterpreter3.png)

Pretty sure after upgrading I probably migrated to a more stable sounding service, don't have a screenshot though.  Since we're using Metasploit, I feel like the next step would be to run local exploit suggester to see what options we have.

![ArticExploitSug.png](./resources/artic/ArticExploitSug.png)

Looks like this system is vulnerable to an it called Schellavator, so lets try that because the name sounds cool.

![ArticSys.png](./resources/artic/ArticSys.png)

And now we're system.  Pretty fun and simple box.
