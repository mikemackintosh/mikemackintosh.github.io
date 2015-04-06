---
layout: post
permalink: /windows-filetime-to-unix
title: "Windows Filetime to Unix"
category: "Windows"
tags: active-directory ldap php-active-directory-time php-timestamp php-windows-time php-windows-timestamp unix-to-windows-time unixtime wft windows-2-unix windows-file-time-to-unix-timestamp windows-filetime windows-ftime windows-time-to-unix windows-time-to-unix-time windows-to-unix-time
---
# Working with Windows Timestamps

When it comes to programming, I spend a lot of time in the world of LDAP, specifically Active Directory. I use it's powers to authenticate, authorize and manage. To do so, I can make a request to the LDAP server, get the user information back and magic logical decision from there. Some fields returned via Active Directory are date/time fields. These fields include `lastpwreset` and `lastlogin`. Unfortunately, Microsoft has implemented some dark age logic, the basis of their nanosecond timestamp since the 1600's.

As most developers and designers are using Linux/Unix boxes for development and hosting, consistency with Unix Timestamps is a must.

# The Functions:

To convert from Windows time to Unix:

    // Windows to Unix Time 
    // @param Windows Timestamp 
    function wftime2unix($timestamp) { 
        return ceil(($timestamp/10000000) - 11644473600); 
    }

To do the reverse, use the below:

    // Unix to Windows Time 
    // @param Unix Timestamp 
    function unix2wftime($timestamp) { 
        return (($timestamp + 11644473600) * 10000000); 
    }

