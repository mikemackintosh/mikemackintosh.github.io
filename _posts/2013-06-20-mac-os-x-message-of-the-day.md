---
layout: post
permalink: /mac-os-x-message-of-the-day
title: "Mac OS X Message of the Day"
category: "How To"
tags: bash cli dynmotd message-of-the-day motd php-2 pty putty sh shell terminal vtty
---
## What is the Message of the Day

Every Linux distribution is unique. One way that they are unique is the shell, and even the message of the day displayed when you login to terminal. Ubuntu 11.10 and higher uses landscape if you are on their server build, OS X shows you the last time you logged in. Some platforms will display a random motivation quote or even show you an ASCII art funny.

By nature, I am CLI driven. I don't mind using a graphical user interface, but I much prefer working through a terminal window. It is much quicker to type than to drag a mouse, and if you are familiar with scripting, I'm sure you agree.

I went ahead and created a PHP script for OS X Mountain Lion (but it does work on 10.6 and up) that will grab some key variables from the `system_profiler` command, and assign them to a variable.

**Note:** I use a mid-2012 Mac Book Pro with Retina, so when you look at the network interfaces, mine may not match yours.

## What does my Message of the Day look like?

Here is a screenshot of what my new message of the day looks like:

![Message of the Day](http://www.highonphp.com/v3/wp-content/uploads/2013/06/Screen-Shot-2013-06-18-at-10.56.35-AM.png)

I've broken it down into different categories:

- Networking
- System
- Battery

There are many options and variables available, such as hard drive space, NIC packets in/packets out, your current display brightness, and more.

## Usage

Installation instructions and the source code is available on GitHub: [mikemackintosh/HackintoshOSX-MOTD](https://github.com/mikemackintosh/HackintoshOSX-MOTD)

Essentially, this is a PHP script, which executes the `system_profiler` command. This command will return output that looks similiar to the below:

    Firewall:
    
    
        Firewall Settings:
    
    
          Mode: Limit incoming connections to specific services and applications
          Services:
              Remote Login (SSH): Allow all connections
              Screen Sharing: Allow all connections
          Applications:
              com.apple.imagent: Allow all connections
              com.parallels.desktop.dispatcher: Allow all connections
          Firewall Logging: Yes
          Stealth Mode: No
    
    
    Graphics/Displays:
    
    
        Intel HD Graphics 4000:
    
    
          Chipset Model: Intel HD Graphics 4000
          Type: GPU
          Bus: Built-In
          VRAM (Total): 512 MB
          Vendor: Intel (0x8086)
          Device ID: 0x0166
          Revision ID: 0x0009
          gMux Version: 3.2.19 [3.2.8]
          Displays:
            Color LCD:
              Display Type: LCD
              Resolution: 2880 X 1800
              Retina: Yes
              Pixel Depth: 32-Bit Color (ARGB8888)
              Main Display: Yes
              Mirror: Off
              Online: Yes
              Built-In: Yes
              Connection Type: DisplayPort
    
    
        NVIDIA GeForce GT 650M:
    
    
          Chipset Model: NVIDIA GeForce GT 650M
          Type: GPU
          Bus: PCIe
          PCIe Lane Width: x8
          VRAM (Total): 1024 MB
          Vendor: NVIDIA (0x10de)
          Device ID: 0x0fd5
          Revision ID: 0x00a2
          ROM Revision: 3688
          gMux Version: 3.2.19 [3.2.8]
    
    
    Hardware:
    
    
        Hardware Overview:
    
    
          Model Name: MacBook Pro
          Model Identifier: MacBookPro10,1
          Processor Name: Intel Core i7
          Processor Speed: 2.7 GHz
          Number of Processors: 1
          Total Number of Cores: 4
          L2 Cache (per Core): 256 KB
          L3 Cache: 6 MB
          Memory: 16 GB
          Boot ROM Version: MBP101.00EE.B03
          SMC Version (system): 2.3f35
          Serial Number (system): ----
          Hardware UUID: ----

The PHP script will the loop through the output and create a variable for each item. You can view a list of the supported variables on the [GitHub page](https://github.com/mikemackintosh/HackintoshOSX-MOTD/blob/master/list_of_vars) as well. It then creates an output similar to a bash script which is received via a pipe. It supports terminal colors for your pleasure.

**Note:** This script is not optimized, but does the job fairly quickly.

