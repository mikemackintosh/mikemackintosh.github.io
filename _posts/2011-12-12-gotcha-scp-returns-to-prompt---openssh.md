---
layout: post
permalink: /scp-returns-to-prompt
title: "Gotcha: SCP Returns To Prompt - OpenSSH"
category: "Networking"
tags: debug1-exit-status-1 linux-scp-failed scp-v-f-ns_sys_config scp-auto-return scp-cli scp-command-line scp-failed-for-11-10 scp-failure scp-failures scp-returns-to-prompt sending-command-scp-v-f-ns_sys_config ubuntu-10-04-scp-failures ubuntu-10-11-scp-failures ubuntu-scp-fail ubuntu-scp-failures ubuntu-ssh-failures
---

### Introduction
On my lab network, I have a box which will reach out to my application gateways and SCP statistics back to my reporting server. I took this idea, and applied it to an audit solution, where I would grab the config of a router/firewall/switch, and return it back to my Audit appliance. It worked great for most devices, but when I ran it against some Juniper ISG's, it hit a brick wall, figuratively. 

## The Issue
The issue is the addition of double dash's in the SCP command, which is sent to the remote device: 

		 scp -v admin@10.2.4.239:ns_sys_config /backup/config/10.2.4.239.cfg 
     Executing: program /usr/bin/ssh host 10.2.4.239, user admin, command scp -v -f -- ns_sys_config 

 The `-v` is used for verbose mode, which is where the excerpt above came from. The `-f` is to tell the destination device to expect a filename, and the `--` is to tell the remote host that all switches have been communicated. Not all SSH capable devices support the `--` feature, resulting in the appending it to the filename. You can correct this by patching the local host which reaches out to these non-compliant devices. Here is some example output of the error. 

 #### Without verbosity (-v): 

		 scp -v admin@10.2.4.239:ns_sys_config /backup/config/10.2.4.239.cfg 
     Password:

 #### With verbosity (-v): 

		 scp -v admin@10.2.4.239:ns_sys_config /backup/config/10.2.4.239.cfg 
     Password:
     debug1: Authentication succeeded (password). 
     debug1: channel 0: new [client-session] 
     debug1: Entering interactive session. 
     debug1: Sending environment. 
     debug1: Sending env LANG = en_US.UTF-8 
     debug1: Sending command: scp -v -f -- /ns_sys_config 
     debug1: client_input_channel_req: channel 0 rtype exit-status reply 0 
     debug1: channel 0: free: client-session, nchannels 1 
     debug1: fd 0 clearing O_NONBLOCK 
     debug1: fd 1 clearing O_NONBLOCK Transferred: sent 2114, received 1524 bytes, in 0.1 seconds Bytes per second: sent 22048.4, received 14831.1 
     debug1: Exit status 1 ... 


## The Patch

I have updated a bug on [bugs.launchpad.net](https://bugs.launchpad.net/debian/+source/openssh/+bug/674390 "OpenSSH Bug") for Ubuntu with a Patch and an explanation. The patch can be directly found [here](https://bugs.launchpad.net/debian/+source/openssh/+bug/674390/+attachment/2593406/+files/scp.patch "SCP Patch"). We will walk through the steps of applying the patch and building.

## Step 1 - The Source
Let's get the source: 

		 wget http://filedump.se.rit.edu/pub/OpenBSD/OpenSSH/portable/openssh-5.9p1.tar.gz 
     tar xvzf openssh-5.9p1.tar.gz 
     cd openssh-5.9p1 

## Step 2 - Apply Patch
Before we do the Configure/Make and Install, we need to patch scp.h, the file which inserts the '--'. 

		 wget https://launchpadlibrarian.net/84923960/scp.patch 
     patch -p1 -i scp.patch # (Stripping trailing CRs from patch.) 
     can't find file to patch at input line 3 Perhaps you used the wrong -p or --strip option? 

The text leading up to this was: 

     -------------------------- 
     |--- scp.c 2011-01-06 06:41:21.000000000 -0500 
     |+++ scp.c 2011-11-11 11:49:39.133830178 -0500 
     -------------------------- File to patch: 

 When it asks you for File to patch, enter `scp.c`. Hit enter.


## Step 3 - Configure/Make/Install
I installed mine at `/usr/local/openssh`. You can change your prefix to wherever you want. 

		 ./configure \
     --prefix=/usr/local/openssh \
     --bindir=/usr/bin \
     --with-ssl-engine \
     --with-ssl-dir=/usr \
     --with-md5-passwords \
     --with-bsd-auth \
     make 
     sudo make install 

 After that, you should be all set! SCP away! Enjoy!