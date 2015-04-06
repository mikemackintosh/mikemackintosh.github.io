---
layout: post
permalink: /move-directory-to-new-partition
title: "Move Directory To New Partition"
category: "How To"
tags: linux lvm partition filesystem
---
### Background - Why Move A Directory

I have a cluster of servers which has a master database server. Originally, the total size of `/` was `32GB`. For the application it was serving, it was ideal; small drive size, less fragmentation, no swap space - a perfect database environment. Some changes went into the application that began logging outputs from remote commands which were run, and the size of the database increased from 200,000 rows to over 5 million in 48 hours. This increase in rows lead to a dramatic increase in size of the database files, causing the server to crash. I had to add a new drive, format, partition and move the database directory to a new filesystem to restore it. These are some of the steps I took.

## Creating the Partition

I inserted a new drive, which took the location of `/dev/sdb`. Since there are no logical or primary partitions on this new drive, the name is `/dev/sdb`. When I started up the box, I chose the `recovery console`, which doesn't load a lot of services and drivers which may be in use on the directory you wish to move. 

To add some partitions, I launched `fdisk /dev/sdb`, and used the option `n` to create a new partition. I selected `Primary` partition, and used the default for sectors since I wanted to use the whole drive. 

    fdisk /dev/sdb 
    Command (m for help): n 

    Command action e extended p primary partition (1-4) p 

    Selected partition 1 First sector (63-134217727, default 63): 
    Using default value 63 
    
    Last sector, +sectors or +size{K,M,G} (63-2047, default 2047): 
    Using default value 2047 

    Command (m for help): w

Now that the partition is created, we have to format it. I chose to use `ext4`. 

    mkfs.ext4 /dev/sdb1

Your new drive is partitioned and formated.

## Mounting and Moving

Next, we need to mount our drive. For this example, I am moving the `/usr/local` directory to this new partition. 

    mv /usr/local /usr/local.bak
    mkdir /usr/local 
    mount -t ext4 /dev/sdb1 /usr/local 
    mv /usr/local.bak/\* /usr/local

## Conclusion

There is one last step we need to take. That is to make sure this new partition is configured to mount on bootup. I opened up `/etc/fstab`, and added a line, which would mount it to the `/usr/local` directory and with filesystem `ext4`. 

Open `_/etc/fstab_`: 
    
    /dev/sdb1 /usr/local ext4 errors=remount-ro 0 1 

After that you can restart your machine if you wish. If you want, you can run a `df -h` and see your new drive listed and it's details: 

    root@splugbox:~# df -h 
    Filesystem Size Used Avail Use% Mounted on 
    /dev/sda1 16G 1.7G 14G 11% / 
    udev 7.8G 4.0K 7.8G 1% /dev 
    tmpfs 3.1G 276K 3.1G 1% /run 
    none 5.0M 0 5.0M 0% /run/lock 
    none 7.8G 0 7.8G 0% /run/shm 
    /dev/sdb1 126G 3.9G 116G 4% /usr/local 
    /dev/sdc1 228M 24M 193M 11% /boot 

Good luck, and post below with any issues. I didn't experience any while using the steps above.