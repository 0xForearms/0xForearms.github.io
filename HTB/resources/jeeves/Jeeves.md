Name: Jeeves  
OS:   Windows   
IP:   10.10.10.63  

Lets start off with an nmap scan  

    sudo nmap -Pn -vv -sV -oN nmap/initial 10.10.10.63

![](./JeevesNmap.png)

Looks like ports 80, 135, 445, and 50000 are open.  Lets take a look at the website to start.

![](./JeevesHTTP1.png)

Nothing promising there, moving on to port 50000 next while I ran a gobuster in the background.

![](./JeevesHTTP2.png)

Nothing on this as well.  Gobustering it as well.

![](./JeevesGoBuster.png)  

Looks promsing, lets visit it.  

![](./JeevesJenkins.png)

Oh Jenkins, I've used this at work for a few automated workflows.  After some brief research of how to get a shell from Jenkins I stumbled upon [this article](https://www.hackingarticles.in/exploiting-jenkins-groovy-script-console-in-multiple-ways/)  

![](./JeevesJenins2.png)
![](./JeevesJenins3.png)

And we've gotten a shell.

![](./JeevesMSF.png)

Went ahead and upgraded to a meterpreter shell.  

![](./JeevesMSF2.png)

And ran local exploit suggester

![](./JeevesLES.png)

ow I seemed that I missed my next screenshot, but it appears I ran mx16_075 with the juicy varient and got a system shell.

![](./JeevesSys.png)

Thanks again for following along.  Until next time.
