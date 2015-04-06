---
layout: post
permalink: /php-ipv4-network-class
title: "PHP: IPv4 Network Class"
category: "Networking"
tags: available-hosts-in-php available-hosts-in-php5 bitwise bitwise-operators get-available-hosts-in-php get-available-hosts-in-php5 how-to-vlsm-in-php how-to-vlsm-in-php5 how-to-vlsm-with-php how-to-vlsm-with-php5 network-management-and-php network-management-and-php5 networking-in-php networking-in-php5 php-and-subnetting php-available-hosts php-bit php-bitwise php-ipv4 php-network-subnet php-networking php-subnet-calculator php-subnetting php-ubuntu-networking php-vlsn php5-and-subnetting php5-available-hosts php5-bit php5-bitwise php5-ipv4 php5-network-subnet php5-networking php5-subnet-calculator php5-subnetting php5-ubuntu-networking php5-vlsn subnet-calulcating subnet-with-php subnet-with-php5 vlsm-php vlsm-php5
---
### Background - Subnetting IPv4 With PHP

For many, upon many, of years, there has been a need to partition and segregate networks based on purpose, need or availability. To do so, engineers and network architects have been employing the use of subnetting. This gives you the ability to divide a larger network into many smaller networks. Although many tools already exist to do this, not many have been adapted for API use. 

**Note:** The class below can be echoed into a JSON response by using the `__toString()` magic method. This method loops through all the methods within the class, and will serialize them into JSON.

## The Class, The Code

You can get a copy of the code below:

{% gist mikemackintosh/5eb2431d6e712be48041 ipv4.php %}

## Example Usage

{% gist mikemackintosh/5eb2431d6e712be48041 example.php %}

Code comments to follow. Share how you have been able to use the script above! Enjoy.