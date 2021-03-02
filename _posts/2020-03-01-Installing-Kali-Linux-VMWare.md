---
title: Installing Kali Linux on VMWare
date: 2020-03-01
permalink: /posts/Installing-Kali-Linux-VMWare
tags:
  - Kali

---  
Kali and Vmware..

# Introduction

![Kali](/images/Kali/kali.png)

Kali Linux is the mostly referred to as the hacker distro of choice, it contains an immense amount of tools needed to conduct penetration tests. Most of the time, you do not want this dedicated to portions of your HDD/SDD as they can be unstable. The best way to do this is via virtual machines, which are designed to virtualize the environment for ease of use. 

# Downloads

* Head to [VMWare](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html) and install their workstation player for our VM.
* Download the Kali ISO image from their [website](https://www.kali.org/downloads/)

# Install

Now that we have both components to set up our virtual machine, all we have to do is select "Create New Virtual Machine" from VMWare Workstation.

![Install1](/images/Kali/Addnew.png)

Here we will browse and select the Kali ISO image we installed earlier.

![Install2](/images/Kali/SelectISO.png)

The rest of the selections are fine as default, and now most of the work is done! You will now complete a Graphical Install of Kali and will set up your desired hostname, password, language, etc.

![Install3](/images/Kali/GraphicalInstall.png)

> A couple things to note, it is easiest to download and use entire disk (allocating all files in a single partition). Make sure to select 'Yes' for network mirror and GRUB boot loader. Lastly, /dev/sda can be used for the boot loader installation.
> 

# Viola!

We now have a virtual machine running Kali Linux. We can pause or run this instance as we please, I will discuss optimizing this image in upcoming posts.

![Viola](/images/Kali/complete.png)



