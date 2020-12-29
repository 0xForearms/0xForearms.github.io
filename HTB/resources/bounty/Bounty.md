Hostname: Bounty  
IP: 10.10.10.93  
OS: Windows  

Just a normal nmap scan. Only has port 80 open.
![](./resources/bounty/59df3c63692787f580628bb03fec0e1a.png)
![](./resources/bounty/72fbcafb8326ed40602e174d7b2342d7.png)

Interesting page, nothing much. Running Nikto/Gobuster to see if we can find anything.

Nikto said it's an asp.net page  
![](./resources/bounty/26f0bdf1515d2da8ff42845484b433ee.png)

restarting gobuster with asp/aspx extensions.  
![](./resources/bounty/3a9cb74ac600a3884acea741184cf618.png)

Couple of interesting pages. Lets check out transfer.aspx  

Looks like a file upload. Lets try to upload a shell.  
![](./resources/bounty/6415134b886ea6f091101451180e7834.png)

Looks like my asp/x files don't upload..  
![](./resources/bounty/47eac27f1d655acbf2a723051610afec.png)

I searched how to bypass an aspx file upload and the first result was promising

![](./resources/bounty/e49778d07c2b57e93307604c7475c445.png)

Uploaded the web.config file they had on the site.
![](./resources/bounty/3362c9c7a08d2aec835f60fd8bcf8680.png)

Looks like it worked, lets check the uploadedfiles directory

![](./resources/bounty/1b401def5dbb6631a8908e40349985b7.png)

According to the script, if it says 3, it's executing code. So lets try to get some command execution.

Couldn't get any text to output to the webpage, but I stumbled upon a website that suggested trying to ping ourselves while having tcpdump running. Something i've seen done before but always forget to try.

![](./resources/bounty/ae122a24210391d6c6f5797379eec4b9.png)

Looks like we have code execution, it's just not visible on the webpage. lets try to get a shell.

![](./resources/bounty/e2092588b5f3e92a98f60a046463ddc0.png)

![](./resources/bounty/979c5af18f0eb705ac2c5e88d91dd7d2.png)

![](./resources/bounty/25759788a7b092b308c29e8527706778.png)

Low level shell. not bad. Lets take a look at this system.
![](./resources/bounty/3ed00dd5908e031fa96860ac8ee88b51.png)

N/A on hotfixes for a Win 08 R2 system? oh no..

Transfered over CVE-2018-8120 and nc.exe
![](./resources/bounty/5666d11bac93a96a6ebca2f3484e794a.png)

Verified the CVE worked. then used it to launch a nc session as system.

![](./resources/bounty/e3ca8ab71528f2a0a3b4a5c9e3945a03.png)

![](./resources/bounty/59d6d08088ad78b2e63e331ba3be65a5.png)
