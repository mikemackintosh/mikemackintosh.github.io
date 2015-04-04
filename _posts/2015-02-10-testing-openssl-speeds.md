---
layout: post
title: "Test OpenSSL Speeds"
category: "How To"
tags: openssl rsa benchmark test
redirect_from:
 - /system-administration/2015/02/10/testing-openssl-speeds/
---

### Introduction

I was recently benchmarking to use of 4096-bit RSA certificates for some secure host communications. One of the servers was a raspberry pi, (lol) and the other was a beast of a machine with 128GB of RAM and 24 cores. Both of these devices would be communicating with each other as clients and servers, so they would each need to verify SSL certs. 


## Benchmarking

Noticing that there was significant lag on the Pi, I wanted to run some benchmarking. Up my sleeve I have a cool trick, which is the `openssl speed` command:

{% gist mikemackintosh/e1061f6ee666dc902160 command.sh %}

You can pass your bit length to the `rsa` so it appears as `rsa4096` if you don't want to test all possible lengths. 

## Interpreting the Results

If you take a look at the results, they provide some very useful results:

{% gist mikemackintosh/e1061f6ee666dc902160 output.sh %}


## Conclusion

As you can see above, we can sustain the signing of 4096-bit rsa's at `44.2` a second. Since this box does not actively sign or serve as a CA, and will only be verifying the certs, let's look at the verify metric. On our current hardware, miraculously, we can verify `2916.7` certs a second signed with rsa4096. This is more than acceptable, since we won't be making more than 1 HTTP request a second.

I'll be posting another article soon about stress testing nginx and SSL offloading. 