Curling
10.10.10.150
Linux

Started off with an nmap scan
    
    sudo nmap -Pn -vv -sV -oN nmap/initial 10.10.10.150

![](./resources/curling/bd5a3fc24f4aea685e02cce25852013f.png)

ssh and webserver, lets look at the webserver
![](./resources/curling/c63c170a80bae71a202f0efa17f9c177.png)

quick check of robots.txt and secret.txt, robots was nothing, but secret was something.
![](./resources/curling/871f29c4e7749640045758a89dfe79e5.png)

Lets try base64 first  
![](./resources/curling/27472f277150709bf303f15686e791f4.png)

Hmm, that looks like a password. Looking at the homepage, one of the articles if signed with 'floris'. Lets try it.
![](./resources/curling/ba4c04e374b892cc99ffacb8f8b85a1e.png

we're in  
![](./resources/curling/451917e9b41b4047a8d07c78a5473086.png)
kinda..

While I was doing this I had gobuster and dirb going in the background. Out of all the stuff it found, one thing stuck out more than the rest, /administrator/

This took me to a joomla admin page, and we know floris works. lets log in
![](./resources/curling/0a91908b34c6442d7a14c6c08a717346.png)
![](./resources/curling/3e61ba777f26030da0c2757e141db21b.png)

Nice! Lets see if we can get a shell. I have limited knowledge of Joomla, so I googled a way to do that and came across this article.
https://www.hackingarticles.in/joomla-reverse-shell/

I followed the steps within that article and...
www-data shell!
![](./resources/curling/1365dc52dea70b0ede1e3d8fe5c8f245.png)

within the home directory of floris we found a 'password_backup'
![](./resources/curling/9fca8ed53b13776b0f1097a7ccf5fa4e.png)

looks to be a hexdump of something. After some messing around
![](./resources/curling/66763b6ce40f34a43ca5fffe20fa7160.png)  
eventually got to  
![](./resources/curling/d5fde11a2937ba221f3ce59b16b2aadd.png)

That sure looks like a password  
![](./resources/curling/b477448f8ea9136bd36d113a5d67614c.png)

And we've got user/user.txt  
![](./resources/curling/91e5718ab006e9141c9694ad47d19dcc.png)

Within his home directory there are a couple of interesting files
![](./resources/curling/98650b7449fd14e0cf4ed343e46dab62.png)

I tried messing with these for a while until I got a nudge to look elsewhere. Looked at the snap version and it appeared vulnerable to CVE-2019-7304. This wasn't out at the time of the boxes release so it's probably unintended, but currently I'm not sure how to do it with curl.

![](./resources/curling/df39015597259cf3a53e9430c1922511.png)
![](./resources/curling/3e04d5769991bc9608e6909c3ab7c619.png)
