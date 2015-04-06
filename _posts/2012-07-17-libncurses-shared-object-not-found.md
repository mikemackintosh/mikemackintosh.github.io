---
layout: post
permalink: /error-loading-shared-libraries-libncurses-so-5-open-shared-object-file-no-file-directory
title: "libncurses Shared Object Not Found"
category: "System Administration"
tags: aptitude cli fedora-2 libncurses libncurses-so libncurses-so-5 mysql-2 ncurses yum
---

### Package Dependencies

Not too long ago, I had the pleasure of working with some contractors who loved linux. Unfortunately, they only used Debian-based distributions like Ubuntu and Mint. As a result, they were not used to the `yum` package manager as their taste leaned more towards aptitude.

With `yum`, different architectures have dedicated packages. For example, an Intel 32-bit platform would use i386 and native 64-bit AMD would use x86_64. If you need support for 32-bit packages on a 64-bit platform, all you need to do is specify it!

This issue is not specific to `libncurses` but being a common package, it is likely frequently appear.

# The Error

    error while loading shared libraries: libncurses.so.5: 
        cannot open shared object file: No such file or directory

# The Fix

You can fix this issue by installing the 32-bit libraries for ncurses. You can do so by running the following command:

    yum install ncurses-devel.i686

# Summary

We hope this helps shed some light on possible `No such file or directory` errors you may encounter.

