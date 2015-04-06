---
layout: post
permalink: /decoding-udp-dns-requests
title: "Decoding UDP DNS Requests"
category: Security
tags: decode-dns-request-packet decode-php decode-php-udp decode-udp dns iptables packet php-2 php-decode-php-request php-dns-request php-sockets php-stream php-udp-decode php5-decode-dns request udp
---

### Introduction

A client presented a scenario where they wanted to be flagged when certain URL's were queried throughout the day, so they could black hole them for their home network. I put together the following workflow for them: 

    Host -> [LAN] -> {FIREWALL} -> (Cloud) 
                        | 
                        | 
                        v 
                Logging Gateway ($gateway) 

We would duplicate all firewall traffic and redirect it to the logging gateway using iptables. 

The 'Gateway' would then listen on the port we wanted to capture traffic for, decode and log. See [the Packet Cloning Article](http://www.highonphp.com/packet-cloning-with-iptables "Packet Cloning") for more details on that. Sample code has been provided below.

## The Code - Decoding DNS

{%gist mikemackintosh/a82b2156fcafc890682b decode_dns.php %}

## The Concept - Bit Deep 
When the UDP packet is received, it's in binary form. We need to take the packets and convert. To do so, we need to take the packet, convert to decimal and then get the ASCII equivalent. If we didn't make this conversion, our output would look like: 

		 10011479510011011545115100495117100112410411510049211010679911110999971151163110101116 

 The use of the PHP function unpack will do just what we need. There are some options where we could unpack to integer format, unsigned long long, and others. We use unsigned char as the output, which then we can use `char()` to convert it to ASCII and output.

## The Result - DNS Log

Here is our beautiful log: 

		 tail /var/log/dns_logger 
         1/29/2012 01:56:15 :: Request for: www.highonphp.com 
         1/29/2012 01:56:28 :: Request for: changelogs.ubuntu.com 
         1/29/2012 01:56:28 :: Request for: changelogs.ubuntu.com.hsd1.nj.comcast.net 
         1/29/2012 01:56:28 :: Request for: changelogs.ubuntu.com 
         1/29/2012 01:57:11 :: Request for: plsordermyfav.pizza 
 
 Enjoy the eat.