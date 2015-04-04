---
layout: post
permalink: /one-liner-get-processor-counts
title: "Get System Processor Counts"
category: "One Liner"
tags: config-management configurations cpu nginx processor-count
---
### Introduction

When you're writing configurations for services like PHP-FPM or nginx, you need to know how many processes are available. One example is the `worker_processes` setting in `nginx.conf`. As you may not be deploying your configuration on similar hardware, it can become cumbersome to manually grab this information for each server. This can be even more of a headache if you manage your configs with Chef or Puppet.

## Querying This Information

Most distributions of Linux will have a `/proc/cpuinfo` file which contains details on the available setup. Well, we can take this output and grep for processor, as every core available will be listed in the output, and then we grab a line count of the results from `grep`:

    cat /proc/cpuinfo |grep processor | wc -l

You can wrap this with `$()` to toss the result into a Bash variable for use in other scripts.

## Example Response

Output of `/proc/cpuinfo` for those curious:

    processor : 0
    vendor_id : GenuineIntel
    cpu family : 6
    model : 2
    model name : QEMU Virtual CPU version 1.0
    stepping : 3
    cpu MHz : 2299.998
    cache size : 4096 KB
    fpu : yes
    fpu_exception : yes
    cpuid level : 4
    wp : yes
    flags : fpu de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pse36 clflush mmx fxsr sse sse2 syscall nx lm up rep_good unfair_spinlock pni vmx cx16 popcnt hypervisor lahf_lm
    bogomips : 4599.99
    clflush size : 64
    cache_alignment : 64
    address sizes : 40 bits physical, 48 bits virtual
    power management:

