---
layout: post
permalink: /manage-growing-innodb-databases
title: "Manage Growing InnoDB Databases"
category: "System Administration"
tags: database export file-size import innodb mysql mysqldump reconcile resize shrink xtradb
---
### InnoDB File Size Management

We are a PerconaDB shop here. We love MySQL, InnoDB and many of the benefits they bring to the table. We have umteen number of articles written for them as well. An intelligently designed database can save you time and money, and a poorly designed one can hurt your bottom line.

## A Few Solutions

**_Note_** : Not all solutions are applicable to your issue. Please use logic and your best judgement in the event one of the below solutions is applicable.

**Solution 1:** Starting off on the right foot

To keep yourself from getting into a sticky situation, it is best to add `innodb_file_per_table` to your `/etc/my.cnf`. What this command does, is tell InnoDB to store table data in separate files, like MyISAM, rather than an uncontrollable `ibdata1` file.

**Solution 2:** Recovering Reclaimed Diskspace

This solution is best done BEFORE you run out of diskspace. The most important note is that you must have enough space on the drive to backup your databases. The reason for this is because we will be exporting ALL your data, dropping your databases, changing the InnoDB storage settings, and then recreating and importing your data.

This is usually a pretty safe process if done during a maintenance window and you are comfortable with databases and your platform.

    $ mysqldump -u<user> -p<pass> --routines --triggers --quick --all-databases > mysql_backup.sql
    $ mysql -u<user> ...
    MySQL Vesion ... yadda yadda
    > drop database <dbname>; /*repeat for all tables */
    > exit
    $ sudo /etc/init.d/mysql stop

At this point, I would encourage you to make the changes provided in Solution 1. Once complete, continue with the below:

    $ sudo rm -rf <path_to_mysql>/data/ib{data1,_log*}
    $ sudo /etc/init.d/mysql start
    $ mysql -u<user> < mysql_backup.sql

If you are not confident with using `rm -rf` within your MySQL directory, you can delete the `ib_logx` files and just rename your `ibdatax` files to `ibdatax.old`. You can delete the backup once your data is reimported.

## Summary

We use this method fairly regularly on some polling systems within our network. We reach around 3 Million tables a month, and the `ibdata1` file has reached upwards of 290GB. Let us know if this came in handy for you!

