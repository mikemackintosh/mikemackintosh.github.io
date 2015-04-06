---
layout: post
permalink: /cross-server-database-pollination
title: "Cross-Server Database Pollination"
category: "MySQL"
tags: automatic-sql-import database manual-replication mysql-2 mysql-export mysql-import mysql-manual-replication mysql-replication mysql-server mysqld mysqldump remote-mysql remote-mysql-export remote-mysql-import remote-sql-import replication server unix-mysql
---
# Background
In my lab, I have two servers running instances of MySQL. One of the servers acted as a development, and the other was staging/production. When playing around with some updates and upgrades for some time killers, I found the need to copy some of the tables automatically from a Cron job, with no user input. I played around with some options such as SSH and replication, but neither were exactly what I was looking for. I ended up using the following which would copy the database table from the remote host and execute it in the active machines mysql space. 

    mysqldump --protocol=TCP -h <remote ip> -u <user> -p'<pass>' <database> <table> | mysql -u <user> -p’<password>' <database>

If you omit the table from the first command, it will export the whole database. 

<b>Note:</b> These two remote servers talk across an encrypted VLAN so I don't stress too much on security here. If you need to, you could always tunnel the MySQL connection to add a simple layer of security.