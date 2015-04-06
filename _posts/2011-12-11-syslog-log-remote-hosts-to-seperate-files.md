---
layout: post
permalink: /syslog-log-remote-hosts-to-seperate-files
title: "Syslog: Log Remote Hosts To Seperate Files"
category: "How To"
tags: change-syslog-file change-syslog-output file-output-syslog new-syslog-file remote-sysklogd remote-syslog remote-syslog-file sysklogd syslog syslog-file syslog-file-out syslog-to-seperate-file syslogd ubuntu-syslog ubuntu-syslog-file
---
### Introduction
I have been making a lot of changes to my lab network, which includes the addition of new networks and honey pots. To save on costs, I purchased a new Linksys router and the first step out-of-box was installing DD-WRT. The steps included below will work with anything that supports remote syslog. 

## My Syslog Environment

| Server Description | Server IP | FQDN |
| --- | --- | --- |
| Syslog Server | 10.1.1.40 | logger.skynet.local.mikemackintosh.com |
| --- | --- | --- |
| Firewall | 10.1.1.1 | firewall.skynet.local.mikemackintosh.com |
| --- | --- | --- |
| Application Server | 10.1.23.42 | v3x.skynet.local.mikemackintosh.com |
| --- | --- | --- |

## Step 1 - Installing Sysklogd

On Deb Based Systems: 

    sudo apt-get install sysklogd

On RPM Based Systems: 

    yum install sysklogd


## Step 2 - Configuring Sysklogd

There are two configuration files for sysklogd. One is installed at `/etc/syslog.conf`. The other, the defaults file, is located at `/etc/default/syslogd`. Open these file in your favorite editor. First, we are going to edit `/etc/default/syslogd`. 

Edit the file to read as: 

    SYSLOGD="-r"

Save. Next, let's open up the other file, `/etc/syslog.conf`. To add support for a remote host by IP, you can append the following to the end of the file: 

    +10.1.1.1 *.* /var/log/firewall.log

After you do, touch the file to create it. 

    sudo touch /var/log/firewall.log 

To listen by hostname, use the following syntax: 

    +v3x.skynet.local.mikemackintosh.com *.* /var/log/appserver.log

## Last Step

Point your hosts to your new syslog server's ip! 

Enjoy!