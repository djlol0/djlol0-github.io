---
title: THM Steel Mountain Writeup
permalink: /posts/THM-Steel-Mountain
tags:
  - TryHackMe

---  
Steel Mountain.../

# [Task 1] - Recon

First we deploy the machine instance. Now let us get an idea of how it is structured.

![Task 1a](/images/THM/SteelMountain/initialnmap.png)

Let's look at the results.

    '''
    Nmap scan report for 10.10.2.233
    Host is up (0.11s latency).
    Not shown: 988 closed ports
    PORT      STATE SERVICE            VERSION
    80/tcp    open  http               Microsoft IIS httpd 8.5
    | http-methods: 
    |_  Potentially risky methods: TRACE
    |_http-server-header: Microsoft-IIS/8.5
    |_http-title: Site doesn't have a title (text/html).
    135/tcp   open  msrpc              Microsoft Windows RPC
    139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
    445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
    3389/tcp  open  ssl/ms-wbt-server?
    | ssl-cert: Subject: commonName=steelmountain
    | Not valid before: 2020-10-11T19:04:29
    |_Not valid after:  2021-04-12T19:04:29
    |_ssl-date: 2021-03-03T04:30:51+00:00; 0s from scanner time.
    8080/tcp  open  http               HttpFileServer httpd 2.3
    |_http-server-header: HFS 2.3
    |_http-title: HFS /
    49152/tcp open  msrpc              Microsoft Windows RPC
    49153/tcp open  msrpc              Microsoft Windows RPC
    49154/tcp open  msrpc              Microsoft Windows RPC
    49155/tcp open  msrpc              Microsoft Windows RPC
    49156/tcp open  msrpc              Microsoft Windows RPC
    49163/tcp open  msrpc              Microsoft Windows RPC
    Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
    '''    
    
And the Host script results: 

    '''
    |_nbstat: NetBIOS name: STEELMOUNTAIN, NetBIOS user: <unknown>, NetBIOS MAC: 02:9a:a1:0f:18:55          (unknown)
    | smb-security-mode: 
    |   account_used: <blank>
    |   authentication_level: user
    |   challenge_response: supported
    |_  message_signing: disabled (dangerous, but default)
    | smb2-security-mode: 
    |   2.02: 
    |_    Message signing enabled but not required
    | smb2-time: 
    |   date: 2021-03-03T04:30:46
    |_  start_date: 2021-03-03T04:24:19
   '''
   
That is a lot of info to digest, so lets look at what ports are open. Http seems to be open on 10.10.2.233.

![Task 1b](/images/THM/SteelMountain/webpage.png)

We see that this guy is employee of the month, but what if you've never seen Mr.Robot? (Said no one). 
There has to be something hiding, lets check the page source. Upon checking you see the name of the employee as a png.

# [Task 2] - Gain Access

We have already scanned the machine with nmap, and we see there is another web server running.
After looking in the page source of this page we have found the vendor of the file server.

![Task 2](/images/THM/SteelMountain/fileserver.png)

Let us search this in msfconsole.

![Task 2a](/images/THM/SteelMountain/searchrejetto.png)

Set our options for current IP config.

![Task 2b](/images/THM/SteelMountain/rejettooptions.png)

Shell into the machine and look for the first flag.

![Task 2c](/images/THM/SteelMountain/billflag.png)        

# [Task 3] - Priv Esc 

Now since we have an inital shell as Bill, we will try to further enumerate and escalate our priv to root.

> PowerUp is used as a 'clearinghouse' of common Windows priv escalation vectors that rely on misconfigurations.

Download the [script here](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1).

Uploading.../
Load powershell../

![loadps](/images/THM/SteelMountain/loadps.png)

> ..\PowerUp.ps1 to get into PowerUp directory 
> Run Invoke-AllChecks

![ASC](/images/THM/SteelMountain/ASCS9.png)

Since the CanRestart option is true, we can restart a service and the directory to the app is also writeable. All in all meaning we can replace the previous application with our own malicious one. Then we stop and restart the service for it to execute. 

Now we generate our payload.

![payload](/images/THM/SteelMountain/payload.png)

Let us upload our binary and replace the legit one.

![upload](/images/THM/SteelMountain/upload.png)

Stop + restart service to gain escalation.

![stopstart](/images/THM/SteelMountain/stopstart.png)

I forgot to show this earlier, but at some point before we restart the ASCS9 service we need to start up a netcat listener to wait for the payload.
> We catch a signal from the executed binary we uploaded

![revshell](/images/THM/SteelMountain/revshell.png)

Search for the root flag .../

![rootflag](/images/THM/SteelMountain/rootflag.png)

# What I learned
* Enumerate all ports when scanning, almost missed 8080 in this case
* If there is a Metasploit module for it, it can be done manually as well
* Read through exploits/scripts before deploying
* When I am uploading images to my website, include the .png tag

