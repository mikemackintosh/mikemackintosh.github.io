---
layout: post
permalink: /mysql-fatal-error-my-print-defaults
title: "Fatal Error: Could not find ./bin/my_print_defaults"
category: "Gotcha"
tags: could-not-find fatal-error my-print-defaults mysql-2 mysql5-5 my_print_defaults percona
---

### Introduction
I was installing new servers in my lab for an application that's about to be deployed. I was taking the normal steps which include MySQL, PHP and Apache which I have documented in the [Web Server Basics article](http://www.highonphp.com/web-server-basics-part-1-an-introduction). While installing Percona, I ran across the following error: 

    # /usr/local/mysql-5.5/bin/mysql_install_db 

    FATAL ERROR: Could not find ./bin/my_print_defaults 

    If you compiled from source, you need to run 'make install' to copy the software into the correct location ready for operation. If you are using a binary release, you must either be at the top level of the extracted archive, or pass the --basedir option pointing to that location.

If you use my article, the MySQL page documents how to circumvent the issue. To get the location of your `my_print_defaults` binary, use the `which` command: 

		 which my_print_defaults /usr/local/mysql-5.5/bin/my_print_defaults

Then you can re-run the `mysql_install_db` script passing the `--basedir` option, leaving off the `/bin/my_print_defaults`: 

	# ./scripts/mysql_install_db --basedir=/usr/local/mysql-5.5 --user=mysql 
    Installing MySQL system tables... 
    120302 10:03:08 [Note] Flashcache bypass: disabled 
    120302 10:03:08 [Note] Flashcache setup error is : ioctl failed OK Filling help tables... 
    120302 10:03:09 [Note] Flashcache bypass: disabled 
    120302 10:03:09 [Note] Flashcache setup error is : ioctl failed OK 

    To start mysqld at boot time you have to copy support-files/mysql.server to the right place for your system 

    PLEASE REMEMBER TO SET A PASSWORD FOR THE MySQL root USER ! 

    To do so, start the server, then issue the following commands: 

        /usr/local/mysql-5.5/bin/mysqladmin -u root password 'new-password' 
        /usr/local/mysql-5.5/bin/mysqladmin -u root -h ubuntu password 'new-password' 

    Alternatively you can run: /usr/local/mysql-5.5/bin/mysql_secure_installation which will also give you the option of removing the test databases and anonymous user created by default. 

    This is strongly recommended for production servers. See the manual for more instructions. 

    You can start the MySQL daemon with: 

        cd /usr/local/mysql-5.5 ; /usr/local/mysql-5.5/bin/mysqld_safe & 

    You can test the MySQL daemon with 

        mysql-test-run.pl cd /usr/local/mysql-5.5/mysql-test ; perl mysql-test-run.pl 

     Please report any problems with the /usr/local/mysql-5.5/scripts/mysqlbug script! 
     Percona recommends that all production deployments be protected with a support contract (http://www.percona.com/mysql-suppport/) to ensure the highest uptime, be eligible for hot fixes, and boost your team's productivity.

Hope this relieves someones headache.