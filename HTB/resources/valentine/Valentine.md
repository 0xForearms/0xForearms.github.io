Valentine  
Linux  
10.10.10.79  

Another one where the name/logo gave something away.  The logo for this bug is a bleeding heart, so I'll be on the lookout for a heartbleed vuln. Anyway, lets start off with an nmap scan.

    sudo nmap -sC -sV -oN nmap/initial 10.10.10.79
    
It found ports 22, 80, and 443 open.  Since heartbleed is a vuln with ssl/tls, lets look at 443 some more.

     sudo nmap --script vuln -p443 -oN nmap/targetedHTTPS 10.10.10.79
     
     Starting Nmap 7.80 ( https://nmap.org ) at 2020-07-28 08:01 CDT
     Nmap scan report for 10.10.10.79
     Host is up (0.039s latency).
     443/tcp open https
     | ssl-heartbleed:
     | VULNERABLE:
     | The Heartbleed Bug is a serious vulnerability in the popular OpenSSL cryptographic software library. It allows for stealing           information intended to be protected by SSL/TLS encryption.
     | State: VULNERABLE
     | Risk factor: High
     | OpenSSL versions 1.0.1 and 1.0.2-beta releases (including 1.0.1f and 1.0.2-beta1) of OpenSSL are affected by the Heartbleed bug. The bug allows for reading memory of systems protected by the vulnerable OpenSSL versions and could allow for disclosure of otherwise encrypted confidential information as well as the encryption keys themselves.
     |
     | References:
     | http://www.openssl.org/news/secadv_20140407.txt
     | http://cvedetails.com/cve/2014-0160/
     |_ https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0160

     Nmap done: 1 IP address (1 host up) scanned in 37.08 seconds

Okay, so it looks vulnverably to Heartbleed, lets dive into that some more.

ran the exploit

![](./fb9cf8650f9133a85d2381aa7755b3ee.png)
![](./4f78ea4207c85ff8a69615f5e841b60f.png)

the text= looks interesting

$text=aGVhcnRibGVlZGJlbGlldmV0aGVoeXBlCg===

![](./d938d1120d0122df58dc95fc408cbe00.png)

Not sure what this is for though. Nmap did have ssh open...
this would have been too easy

![](./74f4ace82da72d4ec8cb4f87461e7917.png)

had gobuster running in the background, it found a few things.

![](./e9abbb507d4872aa898e9f81f58a280e.png)

This looks promising, /dev

![](./e5fd43646f03d35ae3dbdc56b7a56a4f.png)

/encode

![](./901473ca92f2fb2c4977049350ff1b8e.png)

output

![](./e2f3415f40b652839953215ffaf14a2d.png)

decode

![](./ad5a766348231fc5ddaf10844b2f4d1a.png)

Probably could have used those sites to decode the base64 I found but eh...

Now lets go back to the /dev folder. Found this note.

![](./13e1e0c52cff25aac1e5606088f9503d.png)

and this hex dump called hype_key

![](./f7e1fc4845b1702e713bbf6975a5c214.png)

converted from hex to ascii and it appears to be a key

![](./2788a5e892cf6404d5a645f1a92ac5e5.png)

and that text value from earlier came in handy.

![](./deda6cf0172f96a82c7a355b802616ac.png)

quick folder listing of / shows a .devs folder. the preceding . means it's supposed to be hidden

![](./731a12eb4ccd0a384413ffb411c63103.png)

this file looks interesting.

![](./93118508e4bf56dcf0d5e82c293543a3.png)

I'm not sure what a socket is. I googled 'what is a socket file linux' and got a few results

In the tmux post there was a comment about using tmux -S, so after I tried a few of the listed commands I tried that annndddd

![](./ec46362c559396ad0017d323c40351a4.png)

rooted
