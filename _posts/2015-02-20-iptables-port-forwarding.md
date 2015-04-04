---
layout: post
title: "Simple Port Forwarding with IPTables"
category: "How To"
tags: iptables port-forward git ssh
redirect_from:
 - /system-administration/2015/02/20/iptables-port-forwarding/
---

# Background

I run a Git server behind a reverse proxy for HTTP/S traffic. This is all fine and dandy, but causes headaches when you want to use `git:` or `ssh:` remote commands. Because the public IP of the git server is the reverse proxy, and the actual repositories don't live on this box, you have to be creative to get it to work correctly.

The simplest approach to this problem is to setup some port forwarding. In turn, this becomes an SSH reverse proxy as well, without offloading any traffic.

# The Hack

{% gist mikemackintosh/7bb5710a824ab65141ee iptables.sh %}