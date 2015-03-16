---
layout: post
permalink: /decoding-udp-dns-requests
title: "Decoding UDP DNS Requests"
category: ["uncategorized"]
tags: decode-dns-request-packet decode-php decode-php-udp decode-udp dns iptables packet php-2 php-decode-php-request php-dns-request php-sockets php-stream php-udp-decode php5-decode-dns request udp
---
# Intro - Why Log DNS Requests
A client presented a scenario where they wanted to be flagged when certain URL's were queried throughout the day, so they could black hole them for their home network. I put together the following workflow for them: [plain] Host -> [LAN] -> {FIREWALL} -> (Cloud) | | v Logging Gateway ($gateway) [/plain] We would duplicate all firewall traffic and redirect it to the logging gateway using iptables. The 'Gateway' would then listen on the port we wanted to capture traffic for, decode and log. See [the Packet Cloning Article](http://www.highonphp.com/packet-cloning-with-iptables "Packet Cloning") for more details on that. Sample code has been provided below.
# The Code - Decoding DNS
<?php // Let's define some important variables!
$logfile = "/var/log/dns_logger";
$gateway = "10.1.1.40";
$date_format = "n/j/Y H:i:s";

// We are now going to start a UDP Socket Server
// This is going to listen on port 53, DNS
$socket = stream_socket_server("udp://$gateway:53", $errno, $errstr, STREAM_SERVER_BIND);

// If we could not bind successfully, let's throw an error
if(!$socket){
    die($errstr);

}else{
    // Do/While is used to loop through the socket
    // We essentially daemonized it.
    do{
        // When it comes to UDP, we need to use stream_socket_recvfrom.
        // RFC 2671 - 2.3. DNS Messages are limited to 512 octets in size when sent over UDP.
        $pkt = stream_socket_recvfrom($socket, 512, 0, $peer);

        // We are going to strip the first 13 and last 18 bits from packet
        // and unpack with to unsigned Char. The * means repeat for rest of string.
        $pkt = unpack("C*", substr($pkt,13, strlen($pkt)-18));
        $o = '';

        // Now lets loop through the pkt and translate the response to human readable ASCII
        foreach($pkt as $s){
            // If it's less than 32, it's assumed to be a period
            if($s < 32)
                $o .= ".";
            // If its more than 32 less than 127, it's seen as a regular character
            elseif($s > 32 && $s The Concept - Bit Deep When the UDP packet is received, it's in binary form. We need to take the packets and convert. To do so, we need to take the packet, convert to decimal and then get the ASCII equivalent. If we didn't make this conversion, our output would look like: [bash] 10011479510011011545115100495117100112410411510049211010679911110999971151163110101116 [/bash] The use of the PHP function unpack will do just what we need. There are some options where we could unpack to integer format, unsigned long long, and others. We use unsigned char as the output, which then we can use char() to convert it to ASCII and output.
# The Result - DNS Log
Here is our beautiful log: [bash] # tail /var/log/dns\_logger 1/29/2012 01:56:15 :: Request for: www.highonphp.com 1/29/2012 01:56:28 :: Request for: changelogs.ubuntu.com 1/29/2012 01:56:28 :: Request for: changelogs.ubuntu.com.hsd1.nj.comcast.net 1/29/2012 01:56:28 :: Request for: changelogs.ubuntu.com 1/29/2012 01:57:11 :: Request for: plsordermyfav.pizza [/bash] Enjoy the eat.