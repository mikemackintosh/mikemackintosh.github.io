---
layout: post
permalink: /kerberos-libraries-not-found
title: "Building PHP: Kerberos libraries not found"
category: "Gotcha"
tags: php kerberos ubuntu
---
### Adding IMAP Support to PHP

The other day I was working on a project which required PHP to be compiled with IMAP and IMAP-SSL. Normally, this is part of my standard PHP build, but on this platform, I compiled PHP to be as lightweight as possible. Before I rebuilt PHP, I made sure to install `courier-imap` and `courier-imap-ssl` packages as well as `libkrb5-dev`. Kerberos is required for IMAP's extension in PHP.

**Note:** This is an Ubuntu 12.04 64-bit server

When I ran `./configure` on PHP, it kept coming back with nonesense of **_"Kerberos libraries not found_**. This was annoying since I knew my kerberos library files were in `/usr/lib/x86_64-linux-gnu/mit-krb5/`. I made sure the pass this path to `--with-kerberos`, but it still kept failing.

Since I was on a strict timeline to deliver my milestones, I used some standard knowledge of Linux and simply read the output. The output gave references to 3 directories which configure will look for the libraries in as well as a `krb5-config` program which stores many details about kerberos. I took these ideas and applied them, which are documented below.

## Finding Your Kerberos Libraries

If this is the issue you are encountering, you will see the following when you add `--with-kerberos` to your `./configure` for PHP:

    checking for krb5-config... /usr/bin/krb5-config
    configure: error: Kerberos libraries not found.
    
    
          Check the path given to --with-kerberos (if no path is given, searches in /usr/kerberos, /usr/local and /usr )

The problem is that `libkrb5.a|so` is not found in the path you specified, if you specified one. If you did not supply a path, it would look in the defaults of `/usr/kerbero`, `/usr/local` and `/usr`.

Looking at the configure scripts' source code, it appears as if there is a hiccup in this section which doesn't actually check the directory you provide, but instead looks for a `include/` or `libs/` directory within your user-defined path.

If you pay attention to the output, PHP says it found a script called `/usr/bin/krb5-config`. Those familiar with MySQL may be reminded of `mysql-config`, and rightfully so, these commands are simply cousins.

If you execute `krb-config` without any options it will provide a list of options and arguments that it will accept:

    splug@vm:~/php-5.4.14$ krb5-config 
    Usage: /usr/bin/krb5-config [OPTIONS] [LIBRARIES]
    Options:
            [--help] Help
            [--all] Display version, vendor, and various values
            [--version] Version information
            [--vendor] Vendor information
            [--prefix] Kerberos installed prefix
            [--exec-prefix] Kerberos installed exec_prefix
            [--cflags] Compile time CFLAGS
            [--deps] Include dependent libraries
            [--libs] List libraries required to link [LIBRARIES]
    Libraries:
            krb5 Kerberos 5 application
            gssapi GSSAPI application with Kerberos 5 bindings
            kadm-client Kadmin client
            kadm-server Kadmin server
            kdb Application that accesses the kerberos database

Make a note of the `--libs` options. This is where your kerberos library files will be stored on your system.

We will now run the same command but pass this new option:

    splug@vm:~/php-5.4.14$ krb5-config --libs
    -L/usr/lib/x86_64-linux-gnu -Wl,-Bsymbolic-functions -Wl,-z,relro -lkrb5 -lk5crypto -lcom_err

If you look at the response, you will see that the kerberos library files are located in `/usr/lib/x86_64-linux-gnu`.

## Creating a Fix

A simple fix, is link your kerberos files to one of the predetermined paths. We will use our newly discovered path in a symbolic-link command:

    sudo mkdir /usr/kerberos
    sudo ln -s /usr/lib/x86_64-linux-gnu/mit-krb5/* /usr/kerberos

Now, since PHP is looking by default in `/usr/kerberos` for your library files, when you run configure, it will find them with haste.

The next time you run, it will not complain about missing kerberos library files:

    ./configure --with-kerberos

## Summary

It's unfortunate that people don't take the time to read output and perform some command-line-foo. Doing so really does give you the opportunity to learn, understand and troubleshoot more effectively.

