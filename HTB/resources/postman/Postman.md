Name: Postman
OS:   Linux
IP:   10.10.160

I don't remember really anything about this box and I think I did it somehwat recently.  Lets take a look.

    sudo nmap -Pn -vv -sV -oN nmap/initial 10.10.10.160
    and
    sudo nmap -Pn -vv -p- -oN nmap/allTCP 10.10.10.160
    
![](./72bb53f1506ca3a75dcd602b0186750f.png)
![](./5403fc3cf7750685a72f7376f38ff61b.png)
![](./c45060ea37cb28abe23a74781a5fadf5.png)

Looks like ports 22, 80, and 10000, but when we run our all ports scan we also find 6379.

Looking at port 80 we see our homepage.

![](./34e504d2859255e6ec592094e604491a.png)

Checking out port 10000 we get an error but also a redirect

![](./515e291aad8e6e95b850b194df974bbc.png)

Nothing immediate jumped out. However i've added postman to my host files per the note on port 10000

port 6379 stands out as something I haven't seen before. Doing some digging we find this site
https://book.hacktricks.xyz/pentesting/6379-pentesting-redis

Following along the steps with the SSH key, we end up getting a shell

![](./33d68f2449009c90877cdf1503230dda.png)

Ran through a couple quick checks, sudo, passwd, shadow file, other ssh keys and didn't find anything. lets try our enum scripts.

Looks like an interesting file within /opt/  
![](./3a57dbf582b870acf31c153f3e88dafe.png)

Copied the key over, it appears to be encrypted, so lets try john/rock you  
![](./0cb8b8d1eae586f28f3bb5b8625a14c7.png)
![](./f46a9bb8385e866927dd7aba3a56e15c.png)

Looks like we have a way into Matt's account.

I tried the key and the connection would close immediately..  
![](./4c10cb4402da95e0daed3f948d3e7e01.png)

So I tried the passphrase with su back on the redis account, andddd  
![](./95697ed29cfcfabd807e1a590fcb3c97.png)

Progress!
Once again checked a couple of quick things, no sudo ability, no access to shadow/passwd, no password reuse on root. Running linpeas.sh again.

Nothing jumped out to me. Revisitng the enumeration stage, I looked at the webmin server running since it appeared to be started by root.

Checking on an unauthenticated RCE, doesn't appear to work.  
![](./c614f0c5ffbcce0cb6351aef95448c3c.png)

the rest listed in searchsploit are msf modules.

![](./9a0a77054376e715ba7088d4162fed11.png)

![](./14484bbedb74afceabf70b38bd85a9a0.png)

Easypeasy.

I want to revisit something I found earlier while running the linpeas.sh script.  
![](./c615728e00daaba06c214de034e47bfd.png)

tried a few things with mimipenguins, but coudln't get it to work. Looking at the program, it appears this might fall under the limitation/bug area. Maybe i'll dig into this more in my freetime.
