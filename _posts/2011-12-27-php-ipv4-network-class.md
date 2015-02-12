---
layout: post
permalink: /php-ipv4-network-class
title: "PHP: IPv4 Network Class"
category: ["uncategorized"]
tags: available-hosts-in-php available-hosts-in-php5 bitwise bitwise-operators get-available-hosts-in-php get-available-hosts-in-php5 how-to-vlsm-in-php how-to-vlsm-in-php5 how-to-vlsm-with-php how-to-vlsm-with-php5 network-management-and-php network-management-and-php5 networking-in-php networking-in-php5 php-and-subnetting php-available-hosts php-bit php-bitwise php-ipv4 php-network-subnet php-networking php-subnet-calculator php-subnetting php-ubuntu-networking php-vlsn php5-and-subnetting php5-available-hosts php5-bit php5-bitwise php5-ipv4 php5-network-subnet php5-networking php5-subnet-calculator php5-subnetting php5-ubuntu-networking php5-vlsn subnet-calulcating subnet-with-php subnet-with-php5 vlsm-php vlsm-php5
---
# Background - Subnetting IPv4 With PHP
For many, upon many, of years, there has been a need to partition and segregate networks based on purpose, need or availability. To do so, engineers and network architects have been employing the use of subnetting. This gives you the ability to divide a larger network into many smaller networks. Although many tools already exist to do this, not many have been adapted for API use. **Note:** The class below can be echoed into a JSON response by using the \_\_toString() magic method. This method loops through all the methods within the class, and will serialize them into JSON.
# The Class, The Code
[php]<?php / ******************************************
 * IPv4 Network Class
 * Author: Mike Mackintosh
 * Date: 12/27/2011 2121
 *
 * @Usage: new IPv4 ('10.1.1.1', 28);
 *
 ***************************************** /

class IPv4
{
	private $ip_long;
	public $ip;
	public $cidr;
	
	
	function __construct($ip,$cidr)
	{
		$this->ip = $ip; $this->ip\_long = ip2long($ip); $this->cidr = $cidr; } function \_\_toString(){ $methods = get\_class\_methods($this); foreach($methods as $meth){ if(substr($meth, 0, 2) != '\_\_' && $meth != 'mask2cidr' && $meth != 'cidr2mask'){ $r[] = $this->$meth(); } } return json\_encode($r); } static function mask2cidr($mask) { $mask = ip2long($mask); $base = ((1ip; } function cidr() { return $this->cidr; } function netmask() { $netmask = ((1cidr); return long2ip($netmask); } function network() { $network = $this->ip\_long & (ip2long($this->netmask())); return long2ip($network); } function broadcast() { $broadcast = ip2long($this->network()) | (~(ip2long($this->netmask()))); return long2ip($broadcast); } function wildcard() { $inverse = ~(((1cidr)); return long2ip($inverse); } function availablehosts() { $hosts = (ip2long($this->broadcast()) - ip2long($this->network())) -1; return $hosts; } function availablenetworks() { return pow(2, 24)/($this->availablehosts()+2); } } [/php]
# Example Usage
[php] if(isset($\_POST['IP'])){ // Convert back and forth to validate $IP = long2ip(ip2long($\_POST['IP'])); $Mask= long2ip(ip2long($\_POST['Mask'])); try{ if(is\_numeric($\_POST['CIDR'])){ $cidr = $\_POST['CIDR']; }elseif(strlen($Mask) > 4){ $cidr = IPv4::mask2cidr($MASK); }else{ throw new Exception('A valid CIDR or netmask must be provided'); } echo new ipv4($IP,$cidr); } catch(Exception $e){ echo $e->getMessage(); } } [/php] Code comments to follow. Share how you have been able to use the script above! Enjoy.