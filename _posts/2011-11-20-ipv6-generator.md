---
layout: post
permalink: /ipv6-generator
title: "IPv6 Generator"
category: "Networking"
tags: generate-ipv6-addresses generate-ipv6-addresses-ipv6 generate-ipv6-addresses-php ip-v6 ipv6 ipv6-generator ipv6-looping-script ipv6-php-testing-script ipv6-subnet ipv6-subnetting loop-ipv6 networks-ipv6 php-2 php-ipv6-generator subnet-ipv6 v6-php-v6
---
### Background - IPv6

A good systems/security/network administrator has a slew of tools up their sleeves to support their specialty. Unfortunately, many tools still do not support IPv6; those that do put a hurting on your wallet. The below was used to test connectivity across a router using [iptables](http://en.wikipedia.org/wiki/Iptables "iptables"), to make sure the IPv6 addresses were successfully blocked. 

## The IPv6 Generator

    <?php $startime = microtime(true);

    $prefix = "fe80:1234:";
    $octets = 6;
    $fin = pow($octets, 65536);
    echo "Resolving $fin IPv6 Addresses\n\n";

    for($i = 0; $i<= $fin;$i++){
            $oct = dechex($i);
            $addytmp = str_pad($oct, ($octets*3), "0", STR_PAD_LEFT);
            $addytmp = str_split($addytmp, 4);
            $addy = implode(":", $addytmp);
            echo $prefix.$addy."\n";
    }

    echo "It took ".(microtime(true)-$startime)." seconds";

Other Uses You can even do something like the following to test IPv6 ranges for port scanning: 

    <?php $prefix = "fe80:1234:";
    $octets = 6;
    $fin = pow($octets, 65536);

    for($i = 0; $i<= $fin;$i++){
            $oct = dechex($i);
            $addytmp = str_pad($oct, ($octets*3), "0", STR_PAD_LEFT);
            $addytmp = str_split($addytmp, 4);
            $addy = implode(":", $addytmp);
            $fs = fsockopen($addy, 22);
            if($fs){
                  echo $addy." has port 22 open\n"; 
            }

            fclose($fs);
    }

How long does it take you to run the above?

**Update:** I will be updating this script shortly to support automatic detection of provided octets and adding a multiplier to speed it up.