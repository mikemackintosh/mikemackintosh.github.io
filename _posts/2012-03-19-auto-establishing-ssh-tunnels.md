---
layout: post
permalink: /auto-establishing-ssh-tunnels
title: "Auto-Establishing SSH Tunnels"
category: "System Administration"
tags: auto-create-ssh-tunnel auto-establishing create-ssh-tunnel-cronjob cron ssh ssh-tunnel ssh2 tunnel
---
I have a cron job which connects manually replicates a database over an SSH tunnel for those 'Oh Sh!t' moments. Sometimes that SSH tunnel will drop or fail to establish. Within the cron job I needed a way to make sure that didn't happen.

The code below is the first part of the bash file executed by cron. We run `netstat -a` and `grep` for the port the tunnel is supposed to be established on, and if it is less than 2, it will execute and create the tunnel. 

    #!/bin/bash 
    REMOTEHOST=10.1.1.1 
    TUNNEL=$(netstat -a | grep -c 3307) 
    if [$TUNNEL -lt 2]; then 
        ssh -f root@$REMOTEHOST -L 3307:localhost:3306 -N 
    fi 

After the above snippet, you can continue whatever script or application which needed the SSH tunnel to be established. 

Enjoy!