---
layout: post
permalink: /web-server-basics-part-2-apache
title: "Web Server Basics – Part 2: Apache"
category: "How To"
tags: aamp apache-2 apache-from-source apache-from-sourcecode apache-mysql-php apache2 apache2-from-source apache2-2 compile-apache compile-apache-2-2-21 compile-apache-linux compile-apache-so compile-linux-php-apache hamp install-apache-ubuntu-2 lamp linux-apache-mysql-php linux-compile-apache ubunt-11-10-install-apache ubuntu-apache-2 wamp xamp
---

### Introduction - Apache
Since the mid 1990's, Apache has been the most popular HTTP server. There are a few reasons for that, including simplicity, low overhead and it's **FREE**. Installing, configuring and managing an Apache server looks great on a resume, but looks even better when you look back at yourself. Since so many applications and services rely on web servers, it is a good idea to try and install one, to see how and why they work. Following the steps below will walk you through the steps to install Apache from source. For this article, we will be using version 2.2.21.

## Step 1 - Dependencies
Apache, luckily doesn't have any special dependencies, unless you wish to use SSL, which you will need OpenSSL-dev, or OpenSSL-Devel packages.

## Step 2 - Source Code
Let's get the source! 

		 wget http://apache.cs.utah.edu//httpd/httpd-2.2.21.tar.gz 
     tar xvzf httpd-2.2.21.tar.gz 


## Step 3 - Configure/Make/Install


		 cd httpd-2.2.21 
     ./configure \ --prefix=/usr/local/apache-2.2 \ --enable-ssl \ --enable-auth-digest \ --enable-mem-cache \ --enable-so \ --enable-dav \ --enable-rewrite \ --enable-info \ --enable-http \ --enable-suexec \ --enable-cache 
     make 
     sudo make install 


## Step 4 - Postmortem
Similar to MySQL, we need to create a dedicated user for the Apache daemon as well as copy the daemon to our services directory. You can do that by following these steps: 

		 sudo groupadd appadmin 
     sudo useradd -r -g appadmin appadmin 
     sudo cp /usr/local/apache-2.2/bin/apachectl /etc/init.d/apache2 
     export PATH=$PATH:/usr/local/apache-2.2/bin 
 
By default, Apache will use the username and group of `daemon`. We need to manually configure it to use the one which we created above. 

Open `/usr/local/apache-2.2/conf/httpd.conf` and change User and Group (Line 67/68): From: 

		 User daemon 
     Group daemon 

To: 

		 User appadmin 
     Group appadmin 

To start the server, run: 

		 sudo /etc/init.d/apache2 start 

To have the server start on bootup: 

		 sudo update-rc.d apache2 defaults 


## Step 5 - Verify It's Running
Run netstat to check to make sure your system is listening on port 80/www: 

		netstat -a | grep www 

The output should look similar to the following: 

    netstat -a | grep www tcp 0 0 *:www *:* LISTEN 


## Onto PHP

[Continue to Part 3: PHP](http://www.highonphp.com/web-server-basics-part-3-php "Installing PHP from Source")

Enjoy.