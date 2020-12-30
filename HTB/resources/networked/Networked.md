Name: Networked  
OS:   Linux
IP:   10.10.10.146  

Another one from the archives.  This was a fun one as I learned about magic bytes and how to 'hide' files in a sense.  Anyway, lets get started.  

    sudo nmap -Pn -vv -sV -oN nmap/initial 10.10.10.146

![](./2c32f1c7bdd53c46aa130a52ed01aee3.png)

Pretty simple, lets take a look at the webserver on port 80.  While nothing jumped out initially, viewing the source-code gives us a hint.  While digging around, I also made sure to run a gobuster scan in the background.

![](./1f5311c163b141e36482c95972be3cf7.png)
![](./4b2ac0d1f7e10dd4ceccbeabea1c06e8.png)
![](./3668fa9beae2294f2f01990017563bb.png)

Went ahead and downloaded the .tar file and extracted it to review the contents.
d47641d830805a306d6a046252d2ef90.png)
![](./ef9b4496395c098717612858bd173fac.png)

Since we've found an upload page, lets try to upload some stuff.  This took a while to figure out and I don't have many screenshots of this process, but after trying a bunch of file types, I found there was an upload filter that only allowed certain types of images.  Having some knowledge of upload bypass methods I googled for some additional ones and stumbled upon the magic bytes method.  So I used a hexeditor and changed my php file to look like a gif file.

![](./e8c0ad2bb7f0741e830986f6bc3efa87.png)

I sent the upload through Burp to watch the process(and potentially debug anything)

![](./e4285eaf3c28cdab3057f0e959f73601.png)

The site says it uploaded, so lets check.

![](./e1bb591b9f879b2b5a8a2c538419b73f.png)

That looks good, lets try to load our shell. We browsed to our shell via firefox, annndd, got low level access.

![](./286fbe810160a366439d721af80d1ee9.png)

Went ahead and dropped/ran linpeas.sh
![](./ee98112ab6cc7f0d70e7df258e1f6b6a.png)

![](./218160e404826b252684cb9e71f20aa2.png)

Doesn't appear to be running well. Ran through a couple of low hanging fruit commands manually and didn't find anything of note. Reviewing Guly's home directory we find a crontab/php script. Lets review those.

![](./e8cfe750ab9e14f68096df666090c0ae.png)

So it appears that the $value parameter isn't sanitized. Lets pass a command through.
![](./6cf1f7eddcde8c18edb95030ab01d51b.png)

![](./861b9c4dad611ec4ebe08f5574287638.png)

And now i'm guly. Tried a few quick checks and found out I can run changename.sh as sudo. lets take a look
![](./84b9aad0e8f70014bf05c4f9955f9ab6.png)

I did some digging and found this article on Seclists. Such a good site.
https://seclists.org/fulldisclosure/2019/Apr/24

![](./f4b32701735c0e7d1ac0ac622e2f3855.png)  

So lets add bash instead of id and see if it works.  

![](./c2e824650271189e6a25e65dd7351a6c.png)  
