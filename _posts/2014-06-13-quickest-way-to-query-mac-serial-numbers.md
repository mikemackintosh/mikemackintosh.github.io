---
layout: post
permalink: /quickest-way-query-mac-serial-numbers
title: "Quickest Way To Query Mac Serial Numbers"
category: "How To"
tags: ioplatformserialnumber ioreg mac mac-get-serial-number-command-line mac-get-serial-number-terminal optimization performance serial-number system_profiler titan
---
### Introduction

The quickest way to query OS X for a hardware serial number:

    ioreg -c IOPlatformExpertDevice |head -30 |grep IOPlatformSerialNumber | awk '{print $4}' | sed -e s/\"//g

## Querying Serial Numbers

I have been working on a project called [Titan](https://github.com/mikemackintosh/titan), which is a hardening solution written in Python for Mac OS X. It was based off of MIDAS and Tripyarn which was graciously given to the Open-Source Community by the security teams over at Etsy and Facebook.

One of the best identifiers to use in Titan is the hardware serial number. This is found by looking at the Copyright notice on the bottom side of most Apple devices. Luckily, we can also query this using some awesome `cli` commands.

One of the things I love about Apple (but don't call me a fanboy) is their dedication to developers. They have given us many commands, which I will not get into within this post but rather just show you the quickest way to query a serial number for your Mac.

## She's Going For Distance

The first way to grab the serial number through the `cli` would be with the `system_profiler` command. System Profiler will grab a bunch of stats from different data classes on your Mac and organize it for you. You can even export it to XML. This is the source that I suspect `ohai` and `facter` for OS X generate their statistics.

The Serial Number is located inside of the `SPHardwareDataType`:

    $ system_profiler SPHardwareDataType
    Hardware:
    
    
        Hardware Overview:
    
    
          Model Name: MacBook Pro
          Model Identifier: MacBookPro10,1
          Processor Name: Intel Core i7
          Processor Speed: 2.7 GHz
          Number of Processors: 1
          Total Number of Cores: 4
          L2 Cache (per Core): 256 KB
          L3 Cache: 6 MB
          Memory: 16 GB
          Boot ROM Version: MBP101.00EE.B03
          SMC Version (system): 2.3f36
          Serial Number (system): C020000MFF00
          Hardware UUID: C00009D1-0000-5AF6-0000-533130000000

We are going to be extracting the line `Serial Number (system)`. To do this with bash, use:

    system_profiler SPHardwareDataType | grep "Serial Number" | awk '{print $4; }'

If we can prefix this with `time`, we can benchmark how long it takes to run this command:

    time system_profiler SPHardwareDataType | grep "Serial Number" | awk '{print $4; }'
    C020000MFF00
    
    
    real 0m0.179s
    user 0m0.070s
    sys 0m0.037s

## He's Going For Speed

The next command is `ioreg`. This thing is beast. I haven't messed around with it too much, but I have found it to be useful when data mining things off our devices. We can also specify what class of data we want returned to help optimize this, so a command like `ioreg -c IOPlatformExpertDevice` would narrow it down.

    $ ioreg -c IOPlatformExpertDevice
    +-o Root <class IORegistryEntry, id 0x100000100, retain 15>
      +-o MacBookPro10,1 <class IOPlatformExpertDevice, id 0x100000110, registered, matched, active, busy 0 (59297 ms), retain 42>
        | {
        | "compatible" = <"MacBookPro10,1">
        | "version" = <"1.0">
        | "board-id" = <"Mac-C3EC7CD00000081F">
        | "IOInterruptSpecifiers" = (<0900000005000000>)
        | "IOPolledInterface" = "SMCPolledInterface is not serializable"
        | "serial-number" = <464654340000000000000000004330300000000d46465434000000000000000000000000000000000000>
        | "IOInterruptControllers" = ("io-apic-0")
        | "IOPlatformUUID" = "CB31F9D1-9D33-5AF6-99B5-53313425533A"
        | "clock-frequency" = <00e1f505>
        | "IOPlatformSystemSleepPolicy" = <500c500000000000841e020004000000001400...000000>
        | "manufacturer" = <"Apple Inc.">
        | "IOConsoleSecurityInterest" = "IOCommand is not serializable"
        | "IOPlatformSerialNumber" = "C020000MFF00"
        | "system-type" = <02>
        | "product-name" = <"MacBookPro10,1">
        | "model" = <"MacBookPro10,1">
        | "name" = <"/">
        | "IOBusyInterest" = "IOCommand is not serializable"
        | }
        |
        +-o AppleACPIPlatformExpert <class AppleACPIPlatformExpert, id 0x100000111, registered, matched, active, busy 0 (46534 ms), retain 44>
        | +-o IOPMrootDomain <class IOPMrootDomain, id 0x100000114, registered, matched, active, busy 0 (10 ms), retain 72>
        | | +-o IORootParent <class IORootParent, id 0x100000115, !registered, !matched, active, busy 0, retain 7>
        | | +-o RootDomainUserClient <class RootDomainUserClient, id 0x1000002fa, !registered, !matched, active, busy 0, retain 5>
        | | +-o RootDomainUserClient <class RootDomainUserClient, id 0x1000002fb, !registered, !matched, active, busy 0, retain 5>
        | | +-o RootDomainUserClient <class RootDomainUserClient, id 0x1000002ff, !registered, !matched, active, busy 0, retain 5>
        | | +-o RootDomainUserClient <class RootDomainUserClient, id 0x100000301, !registered, !matched, active, busy 0, retain 5>

To extract the serial number, you can use some command line foo and execute:

    ioreg -c IOPlatformExpertDevice |grep IOPlatformSerialNumber | awk '{print $4}'

Now, let's measure the performance like we did with the `system_profiler` command:

    $ time ioreg -c IOPlatformExpertDevice |grep IOPlatformSerialNumber | awk '{print $4}'nt $4; }'
    "C020000MFF00"
    
    
    real 0m0.033s
    user 0m0.012s
    sys 0m0.028s

This is a HUGE performance increase!

## Optimize This More!

Guess, what? We can! By reducing the lines of code output by `ioreg` we can squeeze even more subsecond time out of this command:

    $ time ioreg -c IOPlatformExpertDevice |head -30 |grep IOPlatformSerialNumber | awk '{print $4}'
    "C020000MFF00"
    
    
    real 0m0.012s
    user 0m0.007s
    sys 0m0.014s

If you look, we are within the tenths of a second for querying the serial number. Now that is a fast way to pull this!

## Update: 2014/06/13

Some people were posting that there is a difference in the serial number output, that the `ioreg` command has `"` around the response. They were right. To trim this, we can pipe to `sed -e s/\"//g`. And guess what, still the fastest.

    $ time ioreg -c IOPlatformExpertDevice |head -30 |grep IOPlatformSerialNumber | awk '{print $4}' | sed -e s/\"//g
    C020000MFF00
    
    
    real 0m0.011s
    user 0m0.006s
    sys 0m0.014s

