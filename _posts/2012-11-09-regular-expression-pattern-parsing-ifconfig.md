---
layout: post
permalink: /regex-pattern-parsing-ifconfig
title: "Regular Expression Pattern Parsing ifconfig"
category: ["coding", "network", "php-coding"]
tags: ifconfig interfaces ip-address mac-address networking parsing php-2 regex
---
\*\*Note: \*\* This regex is designed to skip loopback by default.

    preg_match("/^([A-z]\*\d)\s+Link\s+encap:([A-z]\*)\s+HWaddr\s+([A-z0-9:]\*).\*". 
               "inet addr:([0-9.]+).\*Bcast:([0-9.]+).\*Mask:([0-9.]+).*". 
               "MTU:([0-9.]+).\*Metric:([0-9.]+).\*". 
               "RX packets:([0-9.]+).\*errors:([0-9.]+).\*dropped:([0-9.]+).\*overruns:([0-9.]+).\*frame:([0-9.]+).*". 
               "TX packets:([0-9.]+).\*errors:([0-9.]+).\*dropped:([0-9.]+).\*overruns:([0-9.]+).\*carrier:([0-9.]+).*". 
               "RX bytes:([0-9.]+).\*\((.\*)\).*TX bytes:([0-9.]+).\*\((.\*)\)". 
               "/ims", $int, $regex); 
    
    
    if( !empty($regex) ){ 
    
    
        $interface = array(); 
        $interface['name'] = $regex[1]; 
        $interface['type'] = $regex[2]; 
        $interface['mac'] = $regex[3]; 
        $interface['ip'] = $regex[4]; 
        $interface['broadcast'] = $regex[5]; 
        $interface['netmask'] = $regex[6]; 
        $interface['mtu'] = $regex[7]; 
        $interface['metric'] = $regex[8]; 
    
    
        $interface['rx']['packets'] = $regex\[9]; 
        $interface['rx']['errors'] = $regex\[10]; 
        $interface['rx']['dropped'] = $regex\[11]; 
        $interface['rx']['overruns'] = $regex\[12]; 
        $interface['rx']['frame'] = $regex\[13]; 
        $interface['rx']['bytes'] = $regex\[19]; 
        $interface['rx']['hbytes'] = $regex\[20]; 
    
    
        $interface['tx']['packets'] = $regex\[14]; 
        $interface['tx']['errors'] = $regex\[15]; 
        $interface['tx']['dropped'] = $regex\[16]; 
        $interface['tx']['overruns'] = $regex[17]; 
        $interface['tx']['carrier'] = $regex[18]; 
        $interface['tx']['bytes'] = $regex[21]; 
        $interface['tx']['hbytes'] = $regex[22]; 
    
    
        $interfaces[] = $interface; 
    
    
    }

