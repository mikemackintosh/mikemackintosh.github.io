---
layout: post
permalink: /junos-remote-api-scripting
title: "Communicate with the JUNOS Remote API"
category: "How To"
tags: api ex firewall juniper juniper-networks junos-2 mx remote-execution security slax-2 srx xslt
---
### Remotely Executing Commands on JUNOS

Those who are familiar with Juniper and their software known as JUNOS, may not know about the extendability of the platform using SLAX. You can read about SLAX on [Juniper's website](http://www.juniper.net/techpubs/en_US/junos11.2/topics/concept/junos-script-automation-slax-overview.html).

SLAX is a programming language based on XSLT, which allows you to access the API on the JUNOS platform, which ranges from configuration to executing commands via the CLI and conditionally controlling the output. [This SinatraNetworks post](http://blog.sinatranetwork.com/2013/04/11/juniper-srx-op-script-op-monitor/) has a great security-focused SLAX script called `op srx-monitor` and references official documentation from Juniper Networks.

To extend SLAX and JUNOS even further, Juniper's Phil Shafer released [Juise](https://code.google.com/p/juise/).

> JUISE takes the abilities provided by the scripting facility of JUNOS and moves it into the open source world, where a script can run on a remote box, accessing JUNOS resources over the NETCONF (or JUNOScript) API. Initially this will be an excellent environment for creating and debugging scripts, but for many users, it may become their "normal" scripting environment.

The following will document how to configure and install juise and it's dependencies.

## Install lib-ssh2 Dependencies

Honestly, you can do without this one, but it does add some more features. Libssh2 is an open-source SSH2 library with an amazingly easy-to-use API. For the sake of consistency for the rest of the post, we will configure this is a general location so other packages and depend on it.

You can install it by doing the following:

    wget http://www.libssh2.org/snapshots/libssh2-1.4.4-20130507.tar.gz
    tar xvzf libssh2-1.4.4-20130507.tar.gz
    cd libssh2-1.4.4-20130507/
    ./configure --prefix=/usr/local/ssh2 --with-openssl
    make
    sudo make install

## Install libslax Dependency

[libslax](https://code.google.com/p/libslax/) is the library surrounding Juniper's SLAX language. libslax will allow you to execute, debug and validate syntax of SLAX scripts. To install it, complete the following:

    wget https://libslax.googlecode.com/files/libslax-0.14.7.tar.gz
    tar xvzf libslax-0.14.7.tar.gz
    cd libslax-0.14.7/
    ./configure --with-libcurl-prefix=/usr --with-libxslt-prefix=/usr --enable-readline --prefix=/usr/local/slax
    make
    sudo make install

## Install Juise

Follow the following steps to install Juise. It depends on the previous libraries which we built and installed:

    wget https://juise.googlecode.com/files/juise-0.3.21.tar.gz
    tar xvzf juise-0.3.21.tar.gz
    cd juise-0.3.21/
    ./configure --prefix=/usr/local/juise --with-pcre --with-libxml-prefix=/usr --with-libslax-prefix=/usr/local/slax --with-libxslt-prefix=/usr --with-libssh2-prefix=/usr/local/ssh2
    make
    make install

To add Juise to your `$PATH`, run: `export PATH=$PATH:/usr/local/juise/bin`. You can also add this to `.bashrc` or `/etc/profile` so Juise is available across shell sessions.

## Putting it all together

To execute a command with Juise, you can run `juise test.slax`.

**test.slax** :

    version 1.2; 
    
    
    / *************************************************** / 
    /* Example SLAX Script */ 
    /* Written by Mike Mackintosh */ 
    /* mike@highonphp.com */
    /* Credit To Mike Stone */
    /* mstone@juniper.com */
    / *************************************************** / 
    ns junos = "http://xml.juniper.net/junos/*/junos"; 
    ns xnm = "http://xml.juniper.net/xnm/1.1/xnm"; 
    ns jcs = "http://xml.juniper.net/junos/commit-scripts/1.0"; 
    ns ext = "http://xmlsoft.org/XSLT/namespace"; 
    ns str = "http://exslt.org/strings"; 
    ns exsl extension = "http://exslt.org/common"; 
    ns exsl = "http://exslt.org/common"; 
    ns func extension = "http://exslt.org/functions"; 
    ns date = "http://exslt.org/dates-and-times"; 
    
    
    import "junos.xsl";
    
    
    param $host-file; 
    param $env;
    
    
    mvar $result; 
    mvar $connection; 
    mvar $hostname; 
    mvar $password;
    
    
    match / {
       <op-script-results>
          {
    
    
            /* grab remote connection information */
            if (not ($host-file))
                {
                         expr jcs:output("\n\nUsage: juise example.slax [required parameter] [options]\n");
                         expr jcs:output("\tRequired Parameter, specify only one]");
                         expr jcs:output("\t-------------------------------------");
                         expr jcs:output("\tremote-host <ip address> :: Check a single device");
                         expr jcs:output("\thost-file <filename> :: Check all hosts provided in <filename>\n\n");
                         <xsl:message terminate='yes'> "\n\n\n\n";
                }
    
    
            /* if not comparing files, then process the remote-host or host-file parameters */
            var $remote-user = jcs:get-input("Username: ");
    
    
            var $pwd-prompt = "Password: ";
            var $password = jcs:get-secret( $pwd-prompt);
    
    
            if ($host-file)
               {
                 var $hosts = document($host-file);
                 var $host-list = jcs:split("\n",$hosts);
                 for-each($host-list)
                       {
                         if (. != "")
                           {
                             set $connection = jcs:open(.,$remote-user,$password);
    
    
                             if (not (jcs:empty($connection)))
                               { /* valid connection */
    
    
                                 var $config = jcs:execute($connection, "get-configuration" );
                                 expr jcs:output( $config );
    
    
                                 expr jcs:close($connection);
    
    
                               }  
                              else
                                  {
                                    expr jcs:output(concat("Could not connect to ", .));
                                  } /* inner else */
                               } /* outer else */
    
    
                     }
               }
             expr jcs:output( "\n\nDone processing checks.\nExiting.");
          } /* op script result */
       } /*match */

**devices-list.txt** :

    <?xml version="1.0"?>
    <hosts>
    <address>10.211.55.10</address>
    <address>10.211.55.11</address>
    </hosts>

Once you create the two files above, you can execute them with this command:

    juise test.slax host-file devices-list.txt

This script will connect to each device and grab the configuration and return it to the CLI. Since SLAX is XML-based being XSLT, your tags will not be displayed in the output, only the content.

This really opens up the door to some great possibilities.

