---
title: THM Steel Mountain Writeup
permalink: /posts/THM-Steel-Mountain
tags:
  - TryHackMe

---  
Steel Mountian...

# [Task 1] - Recon

![Task 1]()

First we deploy the machine instance. Now let us get an idea of how it is structured.

![Task 1a]()

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

![Task 1b]()

We see that this guy is employee of the month, but what if you've never seen Mr.Robot? (Said no one). 
There has to be something hiding, lets check the page source. Upon checking you see the name of the employee as a png.

# [Task 2] - Gain Access

We have already scanned the machine with nmap, and we see there is another web server running.
After looking in the page source we have found the vendor of the file server.


