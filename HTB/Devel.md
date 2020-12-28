Name:   Devel  
OS:     Windows  
IP:     10.10.10.5  

Another easy level Windows box.  Pretty sure this was another one that I did towards the beginning of my journey so please excuse the Metasploit usage.  Anyway, lets start off with an Nmap scan.

    Sudo nmap -Pn -vv -sV -oN nAPp/initial 10.10.10.5
    
![DevelNmap](./resources/devel/DevelNmap.png)

Looks like ports 21 and 80, ftp and http.  Digging around the homepage of the webserver and it appears to be a basic IIS server, but we also notice that anonymous ftp access is allowed, so lets take a step back and review that.

Since we have anonymous access, we can log in and view the files. Those files happen to be similiar to the root directory of the iis server.  lets see if I can drop a file there and then access it on the server.

![DevelFTP1](./resources/devel/DevelFTP1.png)  
![DevelFTP2](./resources/devel/DevelFTP2.png)  

That's promising.  Lets try to drop a shell.  Going to use aspx because this is a Windows based box and IIS tends to prefer that language(asp or aspx).  

![DevelShell1](./resources/devel/DevelASPXShell.png)  
![DevelShell2](./resources/devel/DevelASPXShell2.png)  
![DevelShell3](./resources/devel/DevelASPXShell3.png)  

Well that doesn't appear to be working.  Lets try a non staged payload.

![DevelShell4](./resources/devel/DevelASPXShell4.png)  
![DevelShell5](./resources/devel/DevelASPXShell5.png)  

Okay, that looks better.  It seems as if we're the iis web user.  Lets move this to Metasploit, because at the time I wasn't good with Windows, and run local exploit suggester.

![DevelLES](./resources/devel/DevelLES.png)  

Looks like quite a few options.  Going to try schellevator first because I've had good luck with it in the past and it sounds cool.  

![DevelPrivEsc1](./resources/devel/DevelPricEsc1.png)  

Welp, that didn't work, lets try the next one, ms15-051

![DevelPrivEsc2](./resources/devel/DevelPricEsc2.png)  

And it appears we are now system.  Sounds like a win in my book.
