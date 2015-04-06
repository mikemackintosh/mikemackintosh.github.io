---
layout: post
permalink: /packet-cloning-with-iptables
title: "Packet Cloning With iptables"
category: "System Administration"
tags: ddwrt iptables logging mangle packet-sniffing sniffing tee traffic-sniffing
---
### Introduction

iptables have been the simplest version of firewalls in the *nix world for years. They allow you to apply NAT to traffic, reconstruct your TTL, drop and/or log and even duplicate interface traffic. We recently had a scenario where we wanted to log **ALL DNS** traffic that floated across our network. Luckily, we have been running [DD-WRT](http://www.dd-wrt.com/site/index "DD-WRT") for years which includes iptables. Until now, we just used the iptables to drop all that non-sense traffic from Asia and set up some nifty forwarding rules to protect our lab and critical services. After finagling with iptables and some extensive ninja-like Googling skills, we were able to find the trigger, ROUTE, and the modifier of `--tee`. A little command line magic, and BINGO! We are sniffing traffic.

## The Setup

    Gateway = 10.1.1.40 
    Host = 10.37.255.29 
    Our LAN = 10.0.0.0/8 
    
    
    Host -> [LAN] -> {FIREWALL} -> (Cloud) 
                          | 
                          | 
                          v 
              Logging Gateway ($gateway)

## The Command

    iptables -t mangle -R PREROUTING 1 -s 10.0.0.0/8 -p udp -j ROUTE --tee --gw 10.1.1.40

## Explanation 

The `-t` option is the target action. Mangle is the indicator which is responsible to altering the packet. Uses could include changing policy based routing, ttl modification, etc. 

The `-R` option is to replace the first rule in the PREROUTING chain. 

The `-s` option defines the source traffic we want to match. For us, it was our whole `/8`. The `-p` option defines the protocol to match. We are going to listen for UDP, since most DNS requests are using UDP. 

The `-j` option defines the action you want to take. Example, we could `DROP`, `ACCEPT`, `REJECT`, `logaccept` or `ROUTE`. 

The `--tee` option is what tee's, or branches, a packet, creating a duplicate. 

The `--gw` option defines the gateway the duplicate packet should be sent to. In our scenario, our Logging Gateway.

## Conclusion

See how to log traffic with this [DNS Logging](http://www.highonphp.com/decoding-udp-dns-requests) example.

