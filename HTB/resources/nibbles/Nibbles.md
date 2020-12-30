Name: Nibbles  
OS:   Linux  
IP:   10.10.10.75  

Another easy linux box.  I don't remember when I did this one, but based off the non zsh version of kali, I'd guess early 2020.  Anyway, lets get started.

    sudo nmap -sC -sV -oN nmap/initial 10.10.10.75
    and
    sudo nmap -Pn -n -sC -sV -O -oN nmap/initial 10.10.10.75
    
![](./3ff3c70b47ed42f885a301b0422a1ec5.png)
![](./8b673c38a92d0a1660e873c9c074dcd5.png)

So ports 22, and 80.  Pretty standard, lets take a look at 80 because the version of SSH is some what up-to-date.

Hello world page, view source has something interesting

![](./0c7d83583584339b5142318951231029.png)

Not much here from manual review. started gobuster

![](./a21857c727b341c4e83aed455cbde557.png)

http://10.10.10.75/nibbleblog/update.php

shows version 4.0.3

![](./78c9d82be8f6187889af3467cb499029.png)

A search of that version lead to multiple Shell Upload articles, including a MS module from R7. Gonna try the manual way first. It appears it's authenticated, so I need to find some creds..

http://10.10.10.75/nibbleblog/content/private/users.xml
shows a username of 'admin' so that's a start

Looked everywhere GoBuster found and didn't see a password. Took a peak at the how to guide to see if it was something silly. Turns out it was set as the name of the box. Probably could have guessed that, but I never think to do that.

![](./665b9959181b9d1b305f873502e34287.png)

So we're in the web app. Lets try that upload exploit from earlier.

![](./435d2c56eed6b46f69893f4889e05eb0.png)

Uploaded, it's in the directory, lets click it

![](./af597ff21f4fa593034ca37df8d147a1.png)

Boom, user shell

![](./8273b8ab1e56b38968042decb49205fb.png)

and user flag

![](./bf3a207274bca2a44d04d7fb38068ecf.png)

Also noticed a 'personal.zip' in the home directory. Could be important.

![](./487e8748e62395318ddff88a0fa8d979.png)

Anywho, lets check some basic Linux enum stuff

sudo -l, got something interesting as well

![](./12cf4343c26460199cad5cac94939dfe.png)

So since that directory didn't exist, I'd try creating it for an easy win before looking into the zip file.

![](./10a2dccf68eaf4b9950a86455f1cae34.png)

That didn't work, but I wanted to try one more thing.  Pretty sure I just appended "/bin/bash" to the monitor.sh script.

![](./04052049620ffd428cf2737d5de3c5de.png)

woo!
