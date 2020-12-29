Hostname: SwagShop
OS: Linux
IP Address: 10.10.10.140
Ports Open: 22,80

Started off with an nmap scan. Found ports 22 and 80 open, so I dug into them some more.
![](./39f0e8ddd8a6f49450099aec5dfaab1e.png)

Going to take a look at the website while running a full port scan in the background.
![](./82344508c202e6e4fb32e75bd557ba99.png)

Looks to be a rather normal online shop site.
![](./0be19f2390e7f883bc651a416677932d.png)

I went ahead and created a user on the site called test.
![](./255b1ec336bd89f868de5ae8120247b4.png)

Started a gobuster directory scan in the background while reviewing the site.

After digging around some folders and files I wasn't making much progress. Eventually stumbled upon a program called MageScan, which can be found here:https://github.com/steverobbins/magescan

This told me it was running version 1.9 which narrowed down the searchsploit results to a few usable entries.

![](./9d5f40819951a10d3ccfee284caadc87.png)
![](./3d84c2f3d4340cf130e190acddbf4acf.png)

Stuggled with the authenticated rce, no luck with it executing properly. Went back to google. Stumbled upon CVE_2015-1397 found here: https://www.cvedetails.com/cve/CVE-2015-1397/ so I googled a github exploit and found one here: https://github.com/joren485/Magento-Shoplift-SQLI

I dropped that script into a file and ran it.
![](./52a3748594842f5fa35b705cb843ae7c.png)
![](./044b01a97dfe1615dc8bab3a414ce6c8.png)

Looks like I now have access to the admin panel.

After futzing around for a while, I stumped upon this site:
(https://blog.scrt.ch/2019/01/24/magento-rce-local-file-read-with-low-privilege-admin-rights/)

In short, i added a way to upload files to the system.

To start go to Catalog > Manage products.
![](./03c3799426b348640cbebaf39aad8f58.png)

Choose one to edit, and select 'Custom Options' on the botto left.

From here I added an option to upload .phtml files
![](./4f0904c15c690d0bc5008099aad8a751.png)
![](./5f556481335eaf2384812dcdb44d913e.png)

Following what the article said, I navigated to the file and added a command
![](./5847fb6d551eaf4eb7c8fc3dec621ad8.png)

Moved over to Burp to run more commands. Specifically a url encoded rev shell.
![](./5d2a4992bdb64ac04d5905220b19f050.png)

![](./c5715baa694e1e34af33bfdeb3c65dc9.png)

Cool, so I have a low level shell.

Checking some basic priv esc methods
![](./ec4f55620ed7573cf21432bf8d1f58c0.png)

I can run vi with a wildcard location.. I think I can exploit this somehow.

![](./1ccc6a4f6f3917f1e4c5f7f29c0f61cd.png)

hit enter, and I should be root..
![](./36bb43533e8af2dd3ee7c2bc40938f18.png)
