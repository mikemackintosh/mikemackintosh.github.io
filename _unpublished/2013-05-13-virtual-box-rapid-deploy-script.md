---
layout: post
permalink: /?p=1149
title: "Virtual Box Rapid-Deploy Script"
category: ["linux-system-administration", "system-administration", "ubuntu-linux-system-administration"]
tags: cloud deploy headless virtual-box vm
---
# The Need For Virtual Rapid-Deployment

We manage hundreds of physical servers. Each server is dedicated to a client, and only we have access to the physical host. Clients will tell us how many virtual instances they want, and we toss it through a simple script. Well, today, we are feeling in the giving way. We published the script on GitHub.

# Breakdown of the Script

The first thing we do is define the Galaxy, Host and VM name. FQDN's don't always need to be resolvable, but they do need to look pretty.

    # bAddInstance alphacenturi
    Are you sure you want to create alphacenturi.andromeda.snet? [n/Y]

This takes the hostname of the machine and appends it to your new virtual machines hostname.

After you select yes, there are a few steps that occur automatically. These include:

1. Create a logical disk, SATA, with 40GB
2. Create an IDE drive, DVD Drive
3. Insert Ubuntu\_x64 12.10.iso into the drive
4. Enable VRDP/E
5. Set Memory Size
6. Start VM

# Options

The following options can be defined to adjust the outcome of the platform:

    --mem Size of the memory, default 1GB
    --hd Size of the hard drive, default 40GB
    --os Operating System to automatically launch, 
                     default Ubuntu_64 (12.10-server)

# Installing the OS

After you have created the new virtual host, you need to configure the operating system that will run on it.

To do so, you can use a VNC viewer such as [RealVNC](http://www.realvnc.com/) or Microsoft's Built-In Remote Desktop app, `mstsc`. The script will automatically mount the .iso file so you can install the OS you wish. You can eject it whenever you are done.

# Summary

This script comes in handy if you do a lot of multi-platform testing. Of course, there are plenty like this, and some even calls this a mini-cloud. No thank you. We hate the term Cloud. It is the biggest misconception and mismarketing term used since household CPU's peaked at 3GHz.

We use this to enable the ability for development firms to deploy stable, tested, and quick instances of a machine. Feel free to download and deploy as needed and let us know what you think!

# Fun Fact

We use Galaxies to define clusters, planets to define hosts and stars to define VM's.

