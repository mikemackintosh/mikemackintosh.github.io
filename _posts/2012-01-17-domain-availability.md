---
layout: post
permalink: /domain-availability
title: "Domain Availability"
category: "Asset Management"
tags: check-domain-availability check-domain-names domain fqdn php-availability php-domain-checker tld top-level-domain validate-domain-names
---
### Introduction

A few client's were looking to expand their internet empire, hoping to automate a process to check domain availability. I put together the below script which would do just that for them. It uses the whois servers from [who.is](http://who.is "who.is"), and makes a simple cURL GET request. The response is then parsed for the text, "appears to be available", and the response is returned. Use as you see fit. The below is now being released under GPLv2. It is a quick, but not fool-proof way of using out-of-date whois servers.

## The Code

    function domainAvailability($domain, $tld){ 
        $ch = curl_init("http://who.is/whois/$domain.$tld/");
        curl_setopt($ch, CURLOPT_HEADER, 0);
        curl_setopt($ch, CURLOPT_POST, 0);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    
    
        $output = curl_exec($ch); curl_close($ch);
        if(strstr($output, 'appears to be available')){ 
            return true; 
        } 
    
    
        return false; 
    } 
    
    
    echo "Checking highonphp.com... ". 
    
    
    (domainAvailability('highonphp', 'com') ? 'Available' : 'Taken')."\n"; echo "Checking highonphp.net... ". 
    
    
    (domainAvailability('highonphp', 'net') ? 'Available' : 'Taken')."\n";

## Explanation

The above code does the following:

- **Step 1:** Supply domain and tld
- **Step 2:** Make cURL request to who.is
- **Step 3:** Search response for 'appears to be available'
- **Step 4:** Return true or false

_ **Disclaimer:** Who.is has spam prevention detection enabled on their site. We are not affiliated with who.is and do not encourage abuse of the above script._

