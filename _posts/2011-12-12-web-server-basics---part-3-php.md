---
layout: post
permalink: /web-server-basics-part-3-php
title: "Web Server Basics - Part 3: PHP"
category: "How To"
tags: compile-linux-php-apache compile-php compile-php-from compile-php-from-source compile-php-from-source-on-ubuntu-11-10 configure-php configure-php-from-source configure-php-on-ubuntu high-on-php high-php-performance install-high-on-php install-php-without-deb performance-for-php php-4 php-5-3-5 php-5-3-6 php-5-3-7 php-5-3-8 php-5-4 php-configure php-deb php-dependencies php-from-nothing php-from-scratch php-from-source php-from-source-on-ubuntu php-high-on php-on-ubuntu php-performance php5 php5-3-8 php54 scratch-php
---


### Introduction
PHP is another one of those iconic milestones in the history of the internet. Started by Rasmus Lerdorf back in the mid-90's (similar to Apache), it was originally used to manage his _personal home page_. This set of Perl scripts, made up what was called _PHP Tools_ and was the foundation of PHP3. Over the past couple of years, there have been some significant changes to PHP to really help it mature. It has been transformed into a solid and globally accepted language which can serve many purposes. PHP is not just used to serve dynamic web pages, but is also used in desktop application development using GTK and/or PHAR, system administration using the CLI aspect, and is used to interface with hundreds of systems. With a growing fan-base, will go strong for decades into the future._Updated: Feb 12, 2012_For this article, we will be using version 5.3.10.


## Step 1 - Dependencies
PHP's major dependencies rely on what modules you would like to have supported. The basic config included below, will require the following modules to be installed: 

		 sudo apt-get install build-essential libncurses5-dev imagemagick \ 
     libpcre3-dev libssl-dev autoconf libmemcached-dev libmemcache-dev \ 
     libxml2-dev libxslt1-dev libcurl3 libzip-dev libexif-dev \ 
     libpcre++-dev libmcrypt-dev libmhash-dev libmimelib1-dev \ 
     libcurl4-openssl-dev libpspell-dev toilet libldap2-dev \ 
     libfreetype6-dev libjpeg8-dev bzip2 libbz2-dev libpng12-dev \ 
     libc-client2007e-dev courier-authlib courier-authlib-dev courier-imap \ 
     openssl courier-imap-ssl cmake libtidy-dev bison libsnmp-dev libtool \ 
     libssh2-1-dev libkrb5-dev 


## Step 2 - Source Code
Let's get the source! 

		 wget http://www.php.net/get/php-5.3.10.tar.gz/from/us2.php.net/mirror 
     mv mirror php-5.3.10.tar.gz 
     tar xvzf php-5.3.10.tar.gz 


## Step 3 - Configure/Make/Install


		 cd php-5.3.10 ./configure \ 
     '--prefix=/usr/local/php-5.3' \ 
     '--with-config-file-path=/usr/local/php-5.3/etc' \ 
     '--with-config-file-scan-dir=/usr/local/php-5.3/etc' \ 
     '--disable-static' \ 
     '--disable-debug' \ 
     '--disable-rpath' \ 
     '--enable-cli' \ 
     '--enable-bcmath' \ 
     '--enable-calendar' \ 
     '--enable-ctype' \ 
     '--enable-exif' \ 
     '--enable-ftp' \ 
     '--enable-gd-native-ttf' \ 
     '--enable-mbstring' \ 
     '--enable-sockets' \ 
     '--enable-wddx' \ 
     '--enable-pdo' \ 
     '--enable-soap' \ 
     '--enable-pcntl' \ 
     '--enable-zip' \ 
     '--enable-sysvsem' \ 
     '--enable-sysvshm' \ 
     '--enable-sysvmsg' \ 
     '--with-mcrypt' \ 
     '--with-mhash' \ 
     '--with-mime-magic' \ 
     '--with-pcre-regex=/usr' \ 
     '--with-pspell=/usr' \ 
     '--with-xmlrpc' \ 
     '--with-zlib=/usr' \ 
     '--with-pear' \ 
     '--with-layout=GNU' \ 
     '--with-ldap' \ 
     '--with-zip=/usr' \ 
     '--with-bz2=/usr' \ 
     '--with-snmp' \ 
     '--with-mysql=/usr/local/mysql-5.5' \ 
     '--with-pdo-mysql=/usr/local/mysql-5.5' \ 
     '--with-mysqli=/usr/local/mysql-5.5/bin/mysql_config' \ 
     '--with-phar' \ 
     '--with-apxs2=/usr/local/apache-2.2/bin/apxs' \ 
     '--with-tidy' \ 
     '--with-openssl=/usr' \ 
     '--with-curl' \ 
     '--with-zlib-dir=/usr' \ 
     '--with-xsl' \ 
     '--with-pic' \ 
     '--with-ttf' \ 
     '--with-jpeg-dir=/usr' \ 
     '--with-png-dir=/usr' \ 
     '--with-freetype-dir=/usr' \ 
     '--with-gettext' \ 
     '--with-iconv' \ 
     '--with-imap' \ 
     '--with-kerberos=/usr' \ 
     '--with-imap-ssl=/usr' 
     make 
     sudo make install 




## Step 4 - Postmortem
Once PHP is placed in it's final resting place, `/usr/local/php-5.3`, we can go ahead and link the config and add the binaries to the system path: 

		 sudo cp php.ini-production /usr/local/php-5.3/etc/php.ini 
     export PATH=$PATH:/usr/local/php-5.3/bin 

 The installer has already added the PHP5 module in Apache's config, resulting in all we need to do is add the TypeHandler. Open `/usr/local/apache-2.2/conf/httpd.conf`. Add the following to the `IfModule mime_module` section: 

		 AddType application/x-httpd-php .php 

 To restart the server, run: 

		 sudo /etc/init.d/apache2 restart 


## Step 5 - Verify It's Running
Run `netstat` to check to make sure your system is listening on port `80/www`: 

		 netstat -a | grep www 

 The output should look similar to the following: 

		 netstat -a | grep www tcp 0 0 *:www *:* LISTEN 

 This confirms Apache is running. To check PHP, run: 

		 php -v 

 If you see this, then you are done! 

		 PHP 5.3.10 (cli) (built: Dec 7 2011 07:22:59) Copyright (c) 1997-2011 The PHP Group Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies 



## Gotcha's

**configure: error: Kerberos libraries not found.**  
I ran into the above error on an Ubuntu 11.04 x86_64 box, and had to change the order of the configuration options. Kerberos had to be specified AFTER ALL `imap` extensions. Stick around to see configuration and performance tweaks to get the most out of your server. 

Enjoy.