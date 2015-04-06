---
layout: post
permalink: /testing-remote-syslog-connectivity
title: "Testing Remote Syslog Connectivity"
category: "How To"
tags: 514 logging perl port remote syslog syslogng udp
---
## Remote Syslog

Many large networks and companies will pass their syslog messages to a remote server. This serves 2 key purposes, saves resources on the local device, and allows them to correlate key performance indicators and events across multiple devices. In smaller environments, this usually is not needed.

One problem with remote syslog is that it uses `UDP` port `514`. Since `UDP` is an unreliable protocol, many simple socket tests will return `Successful`, when in actuality connection to the remote socket failed. This can be a nightmare if you are trying to test deployment of syslog to your network or check firewall rules.

The only surefire way to test is to send actual syslogs. This sounds like a great idea, but if you are in a scenario where you don't want to make the changes live, you're out of luck. Until now.

The following code is written in Perl which allows you to test syslog connectivity without changing system configuration.

### The Source Code

This source uses the `Sys::Syslog` class which can be found on [CPAN: Sys::Syslog](http://search.cpan.org/~rgarcia/perl-5.9.3/ext/Sys/Syslog/Syslog.pm).

**_File:_** syslogtest

    # Includes
    use Sys::Syslog; # all except setlogsock(), or: 
    use Sys::Syslog qw(:DEFAULT setlogsock); # default set, plus setlogsock() 
    use Sys::Syslog qw(:standard :macros); # standard functions, plus macros
    
    
    # Define Vars
    my $host = "10.255.11.3";
    my $facility = "LOCAL0";
    my $ident = "ZypIO";
    
    
    # Instruct it to use UDP
    setlogsock('udp'); 
    
    
    # Set Syslog Host IP
    $Sys::Syslog::host = $host; 
    
    
    # Open log
    openlog($ident, 'ndelay,pid', $facility); 
    
    
    # Write to log
    syslog('info', 'something happened over here'); 
    
    
    # You can pass the first arg using $ARGV[0] instead
    # syslog('info', $ARGV[0]); 
    
    
    # Close log
    closelog;

For the `openlog` command, it takes 3 parameters:

- **ndelay** : Open the connection immediately (normally, the connection is opened when the first message is logged).
- **nowait** : Don't wait for child processes that may have been created while logging the message. (The GNU C library does not create a child process, so this option has no effect on Linux.)
- **pid** : Include PID with each message.

### Usage:

You can simply execute the file above. It will pass the message you defined in the `syslog` statement to the remote server. If you comment that line out, and uncomment the following line, you can pass a message to the file when you execute it, for better interoperability.

Example:

    ./syslogtest "Here is an example message"

This would send the message, "Here is an example message" to the remote syslog server.

Here is a copy of a `tcpdump` showing the packet getting sent:

    2013-07-11 13:26:16.507048 IP (tos 0x0, ttl 64, id 47491, offset 0, flags [DF], proto UDP (17), length 28)
        10.211.55.3.58389 > 10.255.11.3.514: [bad udp cksum 0x57f1 -> 0xc1ee!] [|syslog]
        0x0000: 4500 001c b983 4000 4011 2976 0ad3 3703 E.....@.@.)v..7.
        0x0010: 0aff 0b03 e415 0202 0008 57f1 ..........W.
    2013-07-11 13:26:16.507550 IP (tos 0x0, ttl 64, id 47492, offset 0, flags [DF], proto UDP (17), length 93)
        10.211.55.3.58389 > 10.255.11.3.514: [bad udp cksum 0x5832 -> 0x2181!] SYSLOG, length: 65
        Facility local0 (16), Severity info (6)
        Msg: Jul 11 13:26:16 ZypIO[25377]: something happened over here\0x0a\0x00
        0x0000: 3c31 3334 3e4a 756c 2031 3120 3133 3a32
        0x0010: 363a 3136 205a 7970 494f 5b32 3533 3737
        0x0020: 5d3a 2073 6f6d 6574 6869 6e67 2068 6170
        0x0030: 7065 6e65 6420 6f76 6572 2068 6572 650a
        0x0040: 00
        0x0000: 4500 005d b984 4000 4011 2934 0ad3 3703 E..]..@.@.)4..7.
        0x0010: 0aff 0b03 e415 0202 0049 5832 3c31 3334 .........IX2<134
        0x0020: 3e4a 756c 2031 3120 3133 3a32 363a 3136 >Jul.11.13:26:16
        0x0030: 205a 7970 494f 5b32 3533 3737 5d3a 2073 .ZypIO[25377]:.s
        0x0040: 6f6d 6574 6869 6e67 2068 6170 7065 6e65 omething.happene
        0x0050: 6420 6f76 6572 2068 6572 650a 00 d.over.here..

### Summary

This script will save you time and headache, especially if you are in a crunch. It is easily configured and simple to run/execute. It is also the most reliable way to test syslog over UDP.

