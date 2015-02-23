---
layout: post
permalink: /installing-mysql-engines-post-install
title: "Installing MySQL Engine(s) Post-Install "
category: ["uncategorized"]
tags: federated innodb myisam mysql-2 mysql-5-5 mysql5-5 remote-databases remote-mysql remote-sql-import remote-tables
redirect_from:
 - /installing-mysql-engines-post-install/
 - /installing-mysql-engines-post-install/trackback/
---
# Background - My Need For Federated
I was working on a proof-of-concept the other day and ran into a situation. I separated my databases based off it's perceived functionality. Example, Billing vs Content. The front-end application will only talk to the Content database, while the Billing database is accessed via the backend. This is a perfect concept except for the fact that my Users table was linked to the Billing database but not the Content database. That is an easy fix. MySQL supports an engine called [Federated](http://dev.mysql.com/doc/refman/5.0/en/federated-storage-engine.html "Federated @ MySQL"). The purpose of Federated is to allow access to a remote database/table without the need to synchronization or replication. All you have to worry about is making sure your table descriptions are the same. Find your plugin shared libraries. I know that federated was needed and searched on that: 

    splug@vanilla:~/mysql-5.5.15$ sudo find / -name "\*federated.so" /usr/local/mysql-5.5.15/lib/plugin/ha\_federated.so /home/splug/mysql-5.5.15/storage/federated/ha\_federated.so

Now it's time to install it: 

    //Example:
    mysql> install plugin <name> soname '<filename>.so';

    mysql> install plugin federated soname 'ha_federated.so';


If you see the engine listed, but support is set to 'NO', you are missing the configuration options in <i>my.cnf</i>.

    mysql> show engines;
    +--------------------+---------+------------------------------------------------------------+--------------+------+------------+
    | Engine | Support | Comment | Transactions | XA | Savepoints |
    +--------------------+---------+------------------------------------------------------------+--------------+------+------------+
    | FEDERATED | NO | Federated MySQL storage engine | NULL | NULL | NULL |
    +--------------------+---------+------------------------------------------------------------+--------------+------+------------+
    1 rows in set (0.00 sec)

Open <i>my.cnf</i> in your favorite editor and add 'federated' to the '[mysqld]' portion.

    [mysqld]
    federated

Now check the engines:

    mysql> show engines;
    +--------------------+---------+------------------------------------------------------------+--------------+------+------------+
    | Engine | Support | Comment | Transactions | XA | Savepoints |
    +--------------------+---------+------------------------------------------------------------+--------------+------+------------+
    | FEDERATED | YES | Federated MySQL storage engine | NO | NO | NO |
    +--------------------+---------+------------------------------------------------------------+--------------+------+------------+
    1 rows in set (0.00 sec)

Congrats!