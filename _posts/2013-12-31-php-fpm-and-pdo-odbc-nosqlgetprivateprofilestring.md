---
layout: post
permalink: /php-fpm-pdo-odbc-nosqlgetprivateprofilestring
title: "PHP, FPM and PDO ODBC NoSQLGetPrivateProfileString"
category: "PHP"
tags: php odbc vertica pdo
---
### Introduction

I was working on a project which used `Vertica`, a columnar-based RDBM, which is a pain in the butt to work with on as a sysadmin.

## The Error

The following error was displayed when executing a script from nginx/php-fpm. The same script ran fine from the CLI.

    [S1000][unixODBC][DSI] The error message NoSQLGetPrivateProfileString could not be found in the en-US locale. Check that /en-US/ODBCMessages.xml exists.
    [ISQL]ERROR: Could not SQLConnect.

**_php-fpm.conf_** :

    ...
    env[VERTICAINI] = /opt/vertica/vertica.ini
    env[ODBCINI] = /etc/odbc.ini

I was able to see that there was no ini file being read in for ODBC by debugging the php-fpm process with:

    sudo nohup strace -p {PIDFPM-WORKER}

