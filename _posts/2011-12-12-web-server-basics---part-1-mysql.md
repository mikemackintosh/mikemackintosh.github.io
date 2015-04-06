---
layout: post
permalink: /web-server-basics-part-1-mysql
title: "Web Server Basics - Part 1: MySQL"
category: "How To"
tags: database fedora-mysql innodb install-mysql install-percona install-percona-from-source install-percona-on-ubuntu install-ubuntu-percona-from-source installation-from-source mysql-2 mysql-fast mysql-percona optimize-innodb percona percona-innodb percona-mysql percona-ubuntu ubuntu-mysql
---
### Introduction
A few months ago, I stumbled onto a product called [Percona](http://www.percona.com/ "Percona, MySQL Experts"). Percona is based off MySQL's open source code, so everything you may already be familiar with is still the same. Essentially, it is a matured, nurtured version of MySQL on a sugar rush. The idea behind Percona is to pretty much switch your database engine to InnoDB and tweak some settings.  

**Note:** I skipped a tutorial on installing Linux as I am very distribution agnostic. I love Gentoo, Ubuntu, Mandriva, Fedora and NetBSD; All walks of life. When I talk about installing packages/dependencies, they may not match the naming convention or package name on your system, especially if I refer to them as aptitude packages and you're using yum. I've done my best from memory to accommodate both environments. Any issues, please post a comment below.

## Step 1 - Dependencies
Before we start, we need to make sure that our system has all the required packages installed. The following commands will install what you need: On Deb: 

		sudo apt-get install libncurses5-dev automake libtool cmake g++ build-essential libncurses5-dev bison

 On RPM: 

		sudo yum install ncurses-devel bison-devel automake g++ libtool


## Step 2 - Get The Source
First things first, get the source. You can do so by following this link: [Source Code](http://www.percona.com/redir/downloads/Percona-Server-5.5/Percona-Server-5.5.17-22.1/source/Percona-Server-5.5.17-rel22.1.tar.gz "Percona Source") or by following the steps below on your server. 

		 wget http://www.percona.com/redir/downloads/Percona-Server-5.5/Percona-Server-5.5.17-22.1/source/Percona-Server-5.5.17-rel22.1.tar.gz 
     tar xvzf Percona-Server-5.5.17-rel22.1.tar.gz cd Percona-Server-5.5.17-rel22.1 


## Step 3 - Compile
If you followed the steps above, you have downloaded, extracted and moved into your new directory. Next we need to execute the build file and configure. 

		 sh BUILD/autorun.sh 
     ./configure \
     --prefix=/usr/local/mysql-5.5 \
     --without-plugin-innobase \
     --with-plugin-innodb_plugin 
     make 
     sudo make install 


## Step 4 - Postmortem
After you have completed the steps above, it's time to make some changes to your system to support this new application. 

		 export PATH=$PATH:/usr/local/mysql-5.5/bin 
     sudo groupadd mysql 
     sudo useradd -r -g mysql mysql 
     chmod +x script/mysql_install_db 
     sudo scripts/mysql_install_db --basedir=/usr/local/mysql-5.5 --user=mysql --ldata=/usr/local/mysql-5.5/data 
     sudo cp support-files/my-innodb-heavy-4G.cnf /etc/my.cnf 
     sudo cp support-files/mysql.server /etc/init.d/mysql 
     sudo chmod +x /etc/init.d/mysql 
     sudo chown -R root:root /usr/local/mysql-5.5 
     sudo chown -R mysql:mysql /usr/local/mysql-5.5/data 

 The above commands complete the following tasks:

  1. Create a group called 'mysql'
  2. Create a user 'mysql', assign to group 'mysql'
  3. Execute mysql db install, install database schema
  4. Copy configuration file to /etc/my.cnf
  5. Copy server daemon to /etc/init.d/mysql
  6. Change directory
  7. Change owner to root
  8. Change owner of directory 'data' to root
  9. Add mysql binaries to your PATH

If you want MySQL to start automatically on system boot, you can run the following command: 

		sudo update-rc.d mysql defaults

 To start the server, run: 

		 sudo /etc/init.d/mysql start 


## Step 5 - Verify It's Running
Run `netstat` to check to make sure your system is listening on `3306/mysql`: 

		 netstat -a | grep mysql 

 The output should look similar to the following: 

		 netstat -a | grep mysql 
     tcp 0 0 *:mysql *:* LISTEN unix 2 [ACC] STREAM LISTENING 54260 /tmp/mysql.sock 

 The first line shows MySQL is listening on port 3306 for incoming connections, and the second is the Unix socket, used for local connections to the database. 

 **Security Note:** To prevent MySQL from listening on port 3306, add `skip-networking` to your `/etc/my.cnf` file and restart.

## Time for Apache
 [Continue to Part 2: Apache](http://www.highonphp.com/web-server-basics-part-2-apache "Installing Apache from Source")

## Gotacha's
If you run into the following error: 

		FATAL ERROR: Could not find ./bin/my_print_defaults

 Check out this article: [Fatal Error: my_print_defaults](http://www.highonphp.com/mysql-fatal-error-my-print-defaults)

 Enjoy.