---
layout: post
permalink: /patching-mpm-itk-for-apache-2-2-2x
title: "Patching mpm-itk for Apache 2.2.2x"
category: "System Administration"
tags: apache-2 apache-mpm itk mpm mpm-itk prefork worker
---
### What is MPM-iTK?

`apache2-mpm-itk` (just `mpm-itk` for short) is an MPM (Multi-Processing Module) for the Apache web server. `mpm-itk` allows you to run each of your vhost under a separate uid and gid — in short, the scripts and configuration files for one vhost no longer have to be readable for all the other vhosts. To read more, check out the mpm's site: [http://mpm-itk.sesse.net/](http://mpm-itk.sesse.net/)

### Download Now

**Updated 4/16/2012:** 

Download the [Monolithic Patch](http://www.highonphp.com/downloads/apache2.2-mpm-itk-2.2.22.patch) [Here](http://www.highonphp.com/downloads/apache2.2-mpm-itk-2.2.22.patch)

### How To Use

First, we need to `wget` the patch:

    // Enter directory with apache source files 
    wget http://www.highonphp.com/downloads/apache2.2-mpm-itk-2.2.22.patch 

To patch the file, we are going to use the `patch` command:

    patch -p1 

Once patched, updated your configuration and restart Apache.