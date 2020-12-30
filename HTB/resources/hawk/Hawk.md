Name: Hawk
OS: Linux
IP: 10.10.10.102

Started off with a couple of nmap scans.

![](./8eabaa3ee73878634e15b940b8ad27fb.png)
![](./59d3a93ee444e9d03eb343f0cef93ddf.png)

Looking at the first thing in the nmap scan we see that anonymous FTP access is allowed. This is ususally a good thing.

After diggint through the directories and checking out hidden files, we find an interesting sounding drupal file.

![](./7cc7982827ee967cdc76936e0619c684.png)

Un hide it and checked out what is was
![](./0b9206d42b2567c3ebe110265bf590eb.png)

I think i'm doing this right
![](./c1ceb099e0d3f366eef5764d9b7da575.png)

Well I started John to try and crack that, lets move on. The next thing that sticks out to me is the drupal webpage on port 80.

Messing around on the website, I stumbled upon a user creation page. I couldn't create a user, but I could enumerate that 'admin' was a user.
![](./c3b354a64073062c4761bdbcf5a8858d.png)

I've heard of a tool called Droopescan. So lets use this and scan the site.

![](./c672ca57329d8a606b9d4ca8e1b25a9c.png)

Gonna make a note of version 7.58

also making a mental note of a few authenticated RCEs. This could be our route to an initial shell on the box.
![](./5f6db67443c0ad72d00a155a2e05946f.png)

Need to keep looking for creds though

After not getting anywhere with John, I did some addititional research into openssl passwords/cracking them and stumbled upon bruteforce-salted-openssl.py. I installed that and messed with it some

![](./845c49cbc116800b0458196e0a23e07d.png)

Found out I needed to specify the digest as it does md5 by default. So lets try sha-256.

So I took that password and passed it to openssl to decrypt the file and got this.
![](./1d5c81c4f08079d73b0bc17a0ed75eab.png)

So I couldn't get the username from the note to work, however I got the other username I found earlier to work.

![](./a05826b942f9d15d1030972c1fbd6c20.png)

After googling 'drupal reverse shell' I landed on this page
So I followed it along, and I have a low level shell!

![](./2855e2fee66318f41019544c3a935f45.png)

Tried some basic privesc commands and didn't get anywhere. Ran both LinEnum and LinPEAS and also didn't get anywhere. It appears mysql is running, but I don't have creds. Maybe I can find something within some drupal files as that is the only non-standard thing left to explore.

Eventually stumbled upon some creds for mysql within the settings.php file
![](./32e74bfccb92b09ee03d230f02b946de.png)

So lets try those.
Oh! looks like i'm in
![](./88e23eb6033168f2f105fd0a01ae0015.png)

Tried a few exploits againts mysql and had no luck. Also looked through the databases for anything juicy and i'm not finding much. However someone mentioned that I already have enough info, so I tried for some password reuse. Didn't think of this earlier, should have though.

![](./b9746b2666e74677cc69c4e268a8627d.png)

Looks like i'm in, however I'm in python which isn't ideal. lets get back to bash

![](./45863139ae1105d7ae86a538e945d62e.png)

Not having any luck, I looked over my notes and saw that port 8082 had a webserver but gave an error that it's not accessable remotely.

![](./baf08c48f97b205e2380364c0c952879.png)
![](./924f6b464c08925e4b9398ff81f05176.png)

Maybe it's accessable locally?

![](./befefe318fe3faa635ca83b054b59153.png)

It is! Lets see what this is. Might have to look into some sort of forwarding to gain access. Looking at the srouce code from earlier, we find it's copyrighted back in 2014. Lets see if we can find any versions around then.

After some digging, I don't think the copyright gives an accurate version number. So lets set up the port forward and access the site.

![](./792af5c346c37694af1423f8126af476.png)

Didn't get anywhere with that page, kept saying invalid username or could not connect. Did some digging and stumbled upon a H2 exploit on ExploitDB. Downloaded it, reviewed it and ran it against my localhost/forwarded port.

![](./5764b40cbf061f76cc9a3f8497841bf0.png)

Forgot how to get the IP on linux(doh!) but we're root
