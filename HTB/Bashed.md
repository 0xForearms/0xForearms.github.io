Name:   Bashed  
OS:     Linux  
IP:     10.10.10.68  

Once again I started off with a pretty normal nmap scan.  Looks like port 80 was the only open port.  So lets dig into that.

    sudo nmap -sC -sV -oN nmap/initial 10.10.10.68
    
    [sudo] password for kali:
    Starting Nmap 7.80 ( https://nmap.org ) at 2020-07-06 17:05 CDT
    Nmap scan report for 10.10.10.68
    Host is up (0.082s latency).
    Not shown: 999 closed ports
    PORT STATE SERVICE VERSION
    80/tcp open http Apache httpd 2.4.18 ((Ubuntu))
    |_http-server-header: Apache/2.4.18 (Ubuntu)
    |_http-title: Arrexel's Development Site

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 10.14 seconds

Nothing initially promising on the webserver, so I ran a Gobuster to search for txt/php files.  I seem to be missing the screenshot, so here is the command.

    gobuster dir -u http://10.10.10.68 -w /usr/share/seclists/Discovery/Web-Content/directory--x txt,php
    
![BashedSite.png](./resources/bashed/BashedSite.png)

Looks like a few interesting directories, specifically /dev/

Looking at this site, it appears to be an interactive webshell.  I tried a few one liners to get a reverse shell and had no luck.  Thinking outside the box, I was able to download a php shell from my local machine and get access that way.

![BashedPHP.png](./resources/bashed/BashedPHP.png)
![BashedRevShell.png](./resources/bashed/BashedRevShell.png)

Okay, so now we're on the box.  Checking out some basic linux things, I noticed that there is a scripts folder in the / directory.  Lets check this out.

![BashedScript.png](./resources/bashed/BashedScript.png)

So I have read access, but not read/execute.  However running 'sudo -l' we find we can sudo any command as 'scriptmanager'.  So lets essentially change to the 'scriptmanager' user.

![BashedScriptMan.png](./resources/bashed/BashedScriptMan.png)

We also check out the two files in this directory.  It appears that test.py is opening test.txt and writing teting123! into the file.  However since test.txt is owned by root, it tells us that if we get test.py to open a shell, it should be the root user.  

So I echo'd a python revserse shell into test.py 

![BashedTestPY.png](./resources/bashed/BashedTestPY.png)

And now i'm root!

![BashedRoot.png](./resources/bashed/BashedRoot.png)

Thanks for reading, more to come!
