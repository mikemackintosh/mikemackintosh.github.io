---
layout: post
permalink: /5-tips-for-working-with-ipv6-in-php
title: "5 Tips for Working with IPv6 in PHP"
category: "PHP"
tags: 128-bit 32-bit 64-bit bits bitwise broadcast bytes databases-2 dtr_ntop dtr_pton inet_ntop inet_pton integers ip2long ipv4 ipv6 long2ip mysql-2 networks subnetting unsigned
---
### IPv4 vs IPv6

With IPv4, it was simple enough to use [`ip2long`](http://www.php.net/manual/en/function.ip2long.php) and [`long2ip`](http://www.php.net/manual/en/function.long2ip.php). These two functions would translate a dotted decimal address to an integer and the reverse respectively. In IPv6, we don't have such luxury.

An IPv4 address consists of 32-bits, which most operating systems and programming languages are able to natively support. Since 32-bit platforms support unsigned integers between `0` and `4,294,967,295`, which is also the maximum number of IPv4 IP's, working with IP address pragmatically would not exhaust memory and processing capabilities. This would be the equivalent of saying 2<sup>32</sup>. The `ip2long` function only supports integers up to the max of the operating system and architecture.

IPv6 is a different story. Most machines currently in production are on 64-bit architecture and running a 64-bit operating system. The largest unsigned integer possible on 64-bit platforms is `18,446,744,073,709,551,616`, or 2<sup>64</sup>. This falls short of the possible `340,282,366,920,938,463,463,374,607,431,770,000,000` or 2<sup>128</sup> addresses available in IPv6. These numbers are VERY large and cumbersome and it's easy to see some of the restrictions when working with them.

Because of the issues addressing memory management and the simple fact that working with that many bits in a number is cumbersome even for modern day programming languages, IPv6 support and algorithms are commonly misunderstood.

**_Emphasis:_** IPv6 does not have a _broadcast_ per-se. In IPv4, the last available address in a range would be reserved for broadcast. In IPv6, there is no concept of a broadcast, instead it would use a multicast on the link-local for all nodes, `ff02::1`.

## Tip #1: Validating IPv4 and IPv6

This is a very simple and straightforward tip. We have been seeing many people using `strpos( $ip , ":")` to determine if an IPv6 address is identified or `substr_count( $ip , ".") == 3` to validate an IPv4 address.

While both of them are possible, they are not 100% accurate. They can lead to security holes in your code and bigger problems in the long run. These functions will not accurately detect IPv6 running in IPv4 compatibility mode, ie addresses which look like `::127.0.0.1` or `::ffff:10.10.1.1`.

PHP provides a `filter_var` function which can be supplied with up to 3 arguments to filter and validate input. To validate an IP address, you can pass `FILTER_VALIDATE_IP`. To specifically validate IPv4 vs IPv6, for the 3rd flag you can pass `FILTER_FLAG_IPV4` or `FILTER_FLAG_IPV6`.

**_Note:_** It is also good to note that the `filter_var` function can validate email addresses, url, and more. It is a very useful function.

For example:

    if( filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4) ){
        // Yes it's valid IPv4
    }

This is a surefire and safe way while not wasting resources and re-inventing the wheel. You will see a practical use of these filters later on.

## Tip #2: IPv6 Conversions

The following two functions were introduced in PHP 5.1.0, [`inet_pton`](http://php.net/manual/en/function.inet-pton.php) and [`inet_pton`](http://php.net/manual/en/function.inet-ntop.php). Their purpose is to convert human readable IP addresses into their packed `in_addr` representation. Since the result is not pure binary, we need to use the `unpack` function in order to apply bitwise operators.

Both functions support IPv6 as well as IPv4. The only difference is how you unpack the address from the results. With IPv6, you will unpack with contents with `A16`, and with IPv4, you will unpack with `A4`.

To put the previous in a perspective here is a little sample output to help clarify:

    // Our Example IP's
    $ip4= "10.22.99.129";
    $ip6= "fe80:1:2:3:a:bad:1dea:dad";
    
    
    // ip2long examples
    var_dump( ip2long($ip4) ); // int(169239425)
    var_dump( ip2long($ip6) ); // bool(false)
    
    
    // inet_pton examples
    var_dump( inet_pton( $ip4 ) ); // string(4) " c"
    var_dump( inet_pton( $ip6 ) ); // string(16) "ï¿½ ï¿½"

We demonstrate above that the `inet_*` family supports both IPv6 and v4. Our next step will be to translate the packed result into an unpacked variable.

    // Unpacking and Packing
    $_u4 = current( unpack( "A4", inet_pton( $ip4 ) ) );
    var_dump( inet_ntop( pack( "A4", $_u4 ) ) ); // string(12) "10.22.99.129"
    
    
    $_u6 = current( unpack( "A16", inet_pton( $ip6 ) ) );
    var_dump( inet_ntop( pack( "A16", $_u6 ) ) ); //string(25) "fe80:1:2:3:a:bad:1dea:dad"

**_Note_** : The `current` function returns the first index of an array. It is equivelant to saying `$array[0]`.

After the unpacking and packing, we can see we achieved the same result as input. This is a simple proof of concept to ensure we are not losing any data.

## Tip 3: Ready-Made Functions

We are huge proponents of DRY. DRY is a mentality in coding where you _Don't Repeat Yourself_. Functions, classes and more are perfect examples on how to apply the DRY coding ideology. As a result, we created the two functions below to clean-up userland code.

**_Note:_** We are also creating a PHP extension for those that don't wan't userland garble in their source. Updates will be provided as that time draws nearer.

The `dtr_pton` function will apply the logic we learned. It will validate your input and return false/throw an exception on error:

    /**
     * dtr_pton
     *
     * Converts a printable IP into an unpacked binary string
     *
     * @author Mike Mackintosh - mike@bakeryphp.com
     * @param string $ip
     * @return string $bin
     */
    function dtr_pton( $ip ){
    
    
        if(filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4)){
            return current( unpack( "A4", inet_pton( $ip ) ) );
        }
        elseif(filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6)){
            return current( unpack( "A16", inet_pton( $ip ) ) );
        }
    
    
        throw new \Exception("Please supply a valid IPv4 or IPv6 address");
    
    
        return false;
    }

The `dtr_ntop` function is the inverse to the `dtr_pton` function. It will validate your input to make sure it conforms to the A4/A16 byte pattern and return false/throw an exception on error:

    /**
     * dtr_ntop
     *
     * Converts an unpacked binary string into a printable IP
     *
     * @author Mike Mackintosh - mike@bakeryphp.com
     * @param string $str
     * @return string $ip
     */
    function dtr_ntop( $str ){
        if( strlen( $str ) == 16 OR strlen( $str ) == 4 ){
            return inet_ntop( pack( "A".strlen( $str ) , $str ) );
        }
    
    
        throw new \Exception( "Please provide a 4 or 16 byte string" );
    
    
        return false;
    }

Here are some examples using the newly defined functions. First is a successful IPv6 back and forth:

    try{
    
    
        var_dump( dtr_ntop( dtr_pton( "fe80:1:2:3:a:bad:1dea:dad") ) );
        // Returns: 'string(25) "fe80:1:2:3:a:bad:1dea:dad"'
    }
    catch(\Exception $e){
      echo $e->getMessage(). "\n";
    }

Second is a successful IPv4 back and forth

    try{
    
    
        var_dump( dtr_ntop( dtr_pton( "10.22.99.129") ) ); // String too short: Throws 'Please provide a 4 or 16 byte string'
        // Returns: 'string(12) "10.22.99.129"'
    
    
    }
    catch(\Exception $e){
      echo $e->getMessage(). "\n";
    }

Third, we have appended an extra character to the `dtr_ntop` function to make the input an extra byte long:

    try{
    
    
        var_dump( dtr_ntop( dtr_pton( "10.22.99.129").'a' ) ); // String too short: Throws 'Please provide a 4 or 16 byte string'
    
    
    }
    catch(\Exception $e){
      echo $e->getMessage(). "\n";
    }

Lastly, we have an invalid IPv6 address:

    try{
    
    
        var_dump( dtr_ntop( dtr_pton( "ffff:feee:fg::") ) ); // Invalid IP: Throws 'Please supply a valid IPv4 or IPv6 address'
    
    
    }
    catch(\Exception $e){
      echo $e->getMessage(). "\n";
    }

## Tip #4: AND'ing and OR'ing

Here are a few sample formulas used to calculate different aspects of a network:

    v4 Subnet Mask: long2ip( ((1<<32) -1) << (32 - CIDR ) )
    v4 Wildcard: long2ip( ~(((1<<32) -1) << (32 - CIDR )) )
    
    
    Network: IP Address & Mask
    Broadcast: IP Address | ~Mask
    
    
    Available Hosts: Broadcast - Network -1
    v4 Available Networks: 2^24 - Available Hosts +2

Once you have mastered, or at least studied the above, you can obtain network variables from an IP and a CIDR.

Using our `dtr_pton` and `dtr_ntop`, we will be looking for the network and broadcast for `10.22.99.199/28`. To accomplish this, we would use the following code:

    $ip = dtr_pton("10.22.99.199");
    $mask = dtr_pton(long2ip( ((1<<32) -1) << (32 - 28 ) ));
    
    
    var_dump( dtr_ntop( $ip & $mask ) );
    var_dump( dtr_ntop( $ip | ~ $mask ) );

The response would be:

    string(12) "10.22.99.192"
    string(12) "10.22.99.207"

An example with IPv6, `fe80:1:2:3:a:bad:1dea:dad/82`:

    $ip = dtr_pton("fe80:1:2:3:a:bad:1dea:dad");
    $mask = dtr_pton("ffff:ffff:ffff:ffff:ffff:fff0::");
    
    
    var_dump( dtr_ntop( $ip & $mask ) );
    var_dump( dtr_ntop( $ip | ~ $mask ) );

The response would be:

    string(18) "fe80:1:2:3:a:ba0::"
    string(26) "fe80:1:2:3:a:baf:ffff:ffff"

## Tip #5: Working with Databases

If you have a dynamic need to store IP addresses in string notation to your database, your best bet would be a `VARCHAR(39)`. If you want to store the binary version of the IP, use a `VARBINARY(39)`. This allows for enough storage to be provisioned to hold an entire IPv6 address.

If you have the ability to install UDF's (User-Defined Functions) in MySQL, and are running version 5.6.2 or lower, I would highly suggest you download an install these from [WatchMouse: INET6_PTON and INET6_NTOP](http://labs.watchmouse.com/2009/10/extending-mysql-5-with-ipv6-functions/). These functions along with a mask function allow you to store and retrieve the binary interpretation of an IP through MySQL statements.

**_Note:_** MySQL only recently added support for these functions in 5.6.3 and above.

This is a stable and safe way of storing addresses in a database table. As you can see in the examples on WatchMouse's page, you can validate IP's are within a range and even resolve and perform reverse lookups on IPv6 addresses.

If you cannot install UDF's in MySQL, you can always store `dtr_pton`'s output. By doing so, you can perform bitwise operations in your MySQL statement and return the result within your query. This is very helpful for large databases and applications which need IPv6 support.

## Putting It All Together

With our primary focus being on networking, security and device management, we have been been engulfed in IPv6 for quite some time. We have hit the same hurdles as everyone else, and face-palmed just as much. You can download a `.tar.gz` version (coming soon) of all the functions or install the package and stay updated using [Composer](http://getcomposer.org/).

**To install with Composer:**

Add the following require to your `composer.json` file:

    "dtr/ip": "2.2.4.*@dev"

You can find some of the source files for this project on [GitHub: dTR-IP](https://github.com/mikemackintosh/dTR-IP) and on GitHub Pages: [GitHub Pages: dTR-IP](http://mikemackintosh.github.com/dTR-IP/).

