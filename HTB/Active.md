Name: Active  
OS:   Windows  
IP:   10.10.10.100  

Started off with an nmap scan.  I appeared to have ran the below snippet but my screenshot missed it.
  sudo nmap -Pn -vv -sV -oN nmap/initial 10.10.10.100

![ActiveNmap.png](./resources/ActiveNmap.png)


Port 445 stands out as a good one to look at.  Lets look at it some more with smbmap and smbclient.

![Activesmbmap.png](./resources/Activesmbmap.png)

smbmap shows anonymous access to read the 'Replication' share, so lets take a look.

![Activesmbclient.png](./resources/Activesmbclient.png)

There are a few folders within this share, and after digging through them for a bit, two files stuck out and I wanted to check them out in more detail.  'Registry.pol' and 'Groups.xml'.  I can't remember where I knew this from, either a conference talk or previous CTF, but the cpassword located within 'Groups.xml' isn't as secure as one would think.

![ActiveGroupsXML.png](./resources/ActiveGroupsXML.png)

Using gpp-decrypt we can get the password  

![ActiveGPP.png](./resources/ActiveGPP.png)

Now since the account is SVC_TGS, I'm thinking it's a service account and has something to do with the Ticket Granting Service in Kerberos authentication.  Doing some googling of that, we find something called Kerberosting, specifically [this website](https://www.scip.ch/en/?labs.20181011).

So from here we use Impacket's GetUserSPNs.py to enumerate additional users.

![ActiveUserSPN.png](./resources/ActiveUserSPN.png)

Looks like I'm getting a clock skew issue.  Went on a side tangent to fix that and we re-ran it.

![ActiveUserSPN2.png](./resources/ActiveUserSPN2.png)

Looks like we got a hash for an account called 'Administrator'.  That sounds promising.  Lets see if John can tackle this.  

![Activejohn.png](./resources/Activejohn.png)

Sweet!  Got another password.  However I'm not done enumerating some things.  I just want to re-check SMB with the SVC account creds we have.

![ActiveSMB2.png](./resources/ActiveSMB2.png)

The 'users' directory stands out as something potentially juicy.  We can probably get the user flag while we're there.

![ActiveSMBClient2.png](./resources/ActiveSMBClient2.png)

This appears to be a dead end, so lets move on to the next set of credentials we have, the administrator account.  Since we have a username:password combo, lets try and see if we can execute commands.  

![ActiveAdministrator.png](./resources/ActiveAdministrator.png)

Cool, that's promising, lets use psexec to get a shell.

![ActivePSEXEC.png](./resources/ActivePSEXEC.png)

And we're system.  There we go, full access on the box Active.   
