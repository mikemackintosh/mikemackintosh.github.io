---
layout: post
permalink: /barnyard2-mysql-alert-view-for-snort
title: "Barnyard2 MySQL Alert View for Snort"
category: "Security"
tags: barnyard2 ids mysql view security snort
---
### Introduction

I've been working on a Snort project recently and started logging alerts to a MySQL database. The Barnyard2 MySQL schema is great and effective, but since I don't have time in my sprints to rewrite the tool in a way that would work best for me, I just tossed together a quick view. This way, a simple Sinatra app can use active record to pull the data is a simple query rather than a million joins in your app's source code.

## The SQL

Because I only wanted 1 query for both TCP and UDP/Other, I `left join` both of the tables and if the packet protocol is TCP, I will output the data, else null. Some of the columns may not be the best named, as that's the downside to rapid development.

    select
        e.sid,
        e.cid,
        sig.sig_name as rule,
        sig.sig_rev as rule_revision,
        sig.sig_sid,
        sigc.sig_class_id as rule_class_id,
        sigc.sig_class_name as rule_class,
        e.signature as sig,
        e.timestamp as time,
        d.data_payload as payload,
        INET_NTOA(i.ip_src) as pkt_src,
        i.ip_src as ip_src,
        INET_NTOA(i.ip_dst) as pkt_dst,
        i.ip_dst as ip_dst,
        i.ip_proto as protocol_num,
        CASE WHEN i.ip_proto = 6 THEN 'tcp' WHEN i.ip_proto = 17 THEN 'udp' ELSE 'other' END as protocol,
        s.hostname as sensor,
        s.interface as sensor_iface,
        enc.encoding_text as sensor_encoding,
        CASE WHEN i.ip_proto = 6 THEN tcp.tcp_sport ELSE udp.udp_sport END as pkt_sport,
        CASE WHEN i.ip_proto = 6 THEN tcp.tcp_dport ELSE udp.udp_dport END as pkt_dport,
        CASE WHEN i.ip_proto = 6 THEN tcp.tcp_flags ELSE null END as pkt_flags,
        CASE WHEN i.ip_proto = 6 THEN tcp.tcp_res ELSE null END as pkt_tcp_reset,
        CASE WHEN i.ip_proto = 6 THEN tcp.tcp_ack ELSE null END as pkt_tcp_ack,
        CASE WHEN i.ip_proto = 6 THEN tcp.tcp_seq ELSE null END as pkt_tcp_seq
    
    
    from event as e
    
    
    left join data as d on d.cid=e.cid
    left join sensor as s on s.sid = e.sid
    left join encoding as enc on enc.encoding_type=s.encoding
    left join signature as sig on sig.sig_id=e.signature
    left join sig_class as sigc on sigc.sig_class_id=sig.sig_class_id
    
    
    left join iphdr as i on i.cid=e.cid
    left join tcphdr as tcp on tcp.cid=e.cid
    left join udphdr as udp on udp.cid=e.cid

You will also get a column named `sensor_encoding` which will tell you how to decode the `payload` column. Leave this out of the MySQL query to speed it up, especially if you are pushing as many packets as we are.

Enjoy.

