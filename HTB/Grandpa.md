Name:   Grandpa  
OS:     Windows  
IP:     10.10.10.14  

This was another fun Windows box, from what I recall, I did this with Metasploit again.  Pretty sure I did this really early on as well, some of my notes hint at not pivoting to a more stable service after getting a shell.  Anyway, lets get started.  Here is the nmap scan(missing a screenshot, but I have the text).

    sudo nmap -sC -sV -O -oN nmap/initial 10.10.10.14
    Nmap scan report for grandpa.htb (10.10.10.14)
    Host is up (0.030s latency).
    Not shown: 999 filtered ports
    PORT STATE SERVICE VERSION
    80/tcp open http Microsoft IIS httpd 6.0
    | http-methods:
    |_ Potentially risky methods: TRACE COPY PROPFIND SEARCH LOCK UNLOCK DELETE PUT MOVE MKCOL PROPPATCH
    |_http-server-header: Microsoft-IIS/6.0
    |http-title: Under Construction
    | http-webdav-scan:
    | WebDAV type: Unknown
    | Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH
    | Allowed Methods: OPTIONS, TRACE, GET, HEAD, COPY, PROPFIND, SEARCH, LOCK, UNLOCK
    | Server Type: Microsoft-IIS/6.0
    | Server Date: Fri, 24 Jul 2020 14:22:32 GMT
    Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
    Device type: general purpose
    Running (JUST GUESSING): Microsoft Windows 2003|2008|XP|2000 (92%)
    <snip>
    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done at Fri Jul 24 09:17:26 2020 -- 1 IP address (1 host up) scanned in 16.81 seconds  
    
Okay, so port 80 is open, lets take a look at the website.
 
![](./resources/grandpa/b03b1809eaa5c7bd5441ecb382dbea97.png)  
nmap said it was IIS 6.0 with WebDAV. No luck with manual exploits, trying MSF module

![](./resources/grandpa/d528444f87b1cf74539a38da38dbf1b6.png)

Got a meterpreter shell, unsure of who I am

![](./resources/grandpa/95f0a7352921589b6575c8ad666e8ed9.png)

dropped to a system shell and figured out i'm a network service

![](./resources/grandpa/152560b82a5ed12f57383590a9e7a692.png)

getsystem maybe?

![](./resources/grandpa/66056e52825242bf29c37831477f6807.png)

nope.. Gonna try local exploit suggester since I have a MSF shell

![](./resources/grandpa/31eb8ce2cb6b54003067395e508f7aba.png)

found 6, not bad..

![](./resources/grandpa/d9cc8b214ba0a461130d23b0134cf647.png)

none of them worked. I got access denied on them all.. Did some manual exploration and didn't seem to find anything. A quick nudge got me looking back at my initial shell. Migrated to a more stable process and got some better results

![](./resources/grandpa/97439dcd2114d58270a7e25c8f4eb6fc.png)

some local exploits, found 7 this time. Will have to remember to review what process i'm running as.

![](./resources/grandpa/4b57edb75a2a5db7e36ea24135ae8582.png)

Lets try the first one that's vulnerable

![](./resources/grandpa/984d8697c703835c00fe290735497256.png)

Annnddd we're system.
