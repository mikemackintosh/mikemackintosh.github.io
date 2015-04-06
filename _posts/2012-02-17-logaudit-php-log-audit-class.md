---
layout: post
permalink: /logaudit
title: "LogAudit: PHP Log Audit Class"
category: "System Administration"
tags: varlog audit daemon fcaps linux log logaudit multidimensional-arrays namespace php-log-audit php-multidimensional-array-search php5 php5-3 recursive-iterator recursiveiterator security
redirect_from:
 - /php/logaudit
---
I was recently approached by one of my clients who had a breach on one of their servers. Thinking [FCAPS](http://en.wikipedia.org/wiki/FCAPS "FCAPS Explination") (Fault Management, Configuration, Accounting, Performance, Security), I knew the first place to start on the box was the `auth.log` file. Being an Ubuntu 11.04 box, the log files were located at `/var/log/`. Some distributions change the log location, but it's just about against standard to do so. That's where I came up with the idea for LogAudit. 

I made a PHP script that would import the log file and match up the session start and session close entries. This would allow you to see how long a session was allocated for, who the user was, when it started and ended, as well as the module and sub-module which allowed the authentication. I whipped together `LogAudit` within a couple of minutes and had a method of procedure for the customer within an hour. 

Here is an example of what two of the log entries looked like: 

    Aug 23 22:36:53 angryserver sshd[8049]: pam_unix(sshd:session): session opened for user sixeightzero 
    Aug 23 23:19:26 angryserver sshd[8049]: pam_unix(sshd:session): session closed for user sixeightzero 

**Note:** Please not that unless you adjust the permissions on the platform to be readable by the user executing this script, by default, root access is required to run this script.

I recently migrated the code to a `gist`, which you can find below:

{% gist mikemackintosh/db16fe31dcf24983e260 parser.php %}

### Examples 

Here are a few methods built-in: 

{% gist mikemackintosh/db16fe31dcf24983e260 method_examples.php %}

Here is an example of the returned object: 
  
{% gist mikemackintosh/db16fe31dcf24983e260 output_example.sh %}

Enjoy.