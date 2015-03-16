---
layout: post
permalink: /importance-of-make-clean
title: "Importance of Make Clean"
category: system-administration
tags: configure clean configure-from-source make make-clean source source-code
---
Some System Administrators like to change application prefixes around for security, organization and other reasons. Sometimes, being to quick for their fingers, they may forget a step in the install process. Once the source is ready for `make`, your configure options have already been parsed and saved. Running `./configure` multiple times without a `make clean` could cause some unwanted behavior.

In this example, `./configure` was run on Apache source without a `--prefix`. The second time we ran it, we included a prefix of `/usr/local/apache-2.2`. As you can see, the default prefix is `/usr/local/apache2`, which carried over to the second make instance of Apache. 

    /bin/sh /opt/httpd-2.2.22/srclib/apr/libtool --mode=install /usr/bin/install -c -m 755 libaprutil-1.la /usr/local/apache21/lib 
    libtool: install: error: cannot install `libaprutil-1.la' to a directory not ending in /usr/local/apache2/lib make[2]: \*\*\* 
    [install] Error 1 make[2]: Leaving directory `/opt/httpd-2.2.22/srclib/apr-util' 

*Don't be mean, use `make clean`* If you are running configure more than once, make sure you use `make clean` between each `./configure`, `make` and `make install`. 

Enjoy!