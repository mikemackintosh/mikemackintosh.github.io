---
layout: post
permalink: /ssh-tunneling
title: "Create an SSH Tunnel"
category: "Security"
tags: create-ssh-tunnel create-ssh2-tunnel creating-an-ssh-tunnel forward-traffic php-ssh2-tunneling php-tunnel ssh-tunnel ssh-tunneling ssh-tunnels ssh1-tunneling ssh2-forwarding-traffic ssh2-tunnel ssh2-tunneling ssh2-tunnels
---
### Background: The Need For A Tunnel
For hundreds of years, our civilization has been building tunnels to circumvent blocks. In the technical world, the need it still there. When a device sites behind a firewall with specific ports blocked, you can proxy your traffic through a server/host on the outside to reach your destination. Below, you can find some example scenarios and the commands used to accomplish the task. 

**Note**: From a security perspective, the firewall ports and/or networks are blocked for a reason, so you should only do this if you understand the risks and know what you are doing.

## Creating The Tunnel
There was a situation where a client wanted to get to a website, let's use `www.highonphp.com` as an example, but the network was blocked by a firewall. An exception was submitted to their security team, but due to their policy deployment schedule, would have taken 30 days for the change to take effect. They had an external server which could reach the destination, and decided to use that as their proxy. 

### Blocked Port 80/Destination Network

The following command will forward all traffic typed as `http://localhost:8080` to `highonphp.com:80`:

    ssh -f user@remoteserver.com -L 8080:highonphp.com:80 -N

**Note:** If you are redirecting traffic to port 80, and the local port is not 80 or 443, then you may need to specify the 'http://' in your URL so the browser knows what protocol to talk. 

### Tunnel Traffic To The Middle Man

If an SSH connection can be established to a remote host, but other ports are blocked, you can specific `localhost` in the command to have traffic stay on the remote system. For example, if you can SSH to a box and need MySQL access but cannot reach port 3306, the following command will tunnel your MySQL traffic: 

    ssh -f user@remoteserver -L 3307:localhost:3306 -N 

Then, within your script or application such as or [Querious](http://www.araelium.com/querious/?from=highonphp), use host `localhost` and port `3307`.

## Tunnel With PuTTy

PuTTy is one of the best SSH client applications that has ever existed. One of the benefits of PuTTy, is the ability to tunnel traffic from a Windows machine to another remote system. To set up the tunnel, enter the normal connection details and follow the steps below. 

To tunnel traffic from `localhost:8080` to `remotehost:80`, use the following:

  1. Expand _Connection_
  2. Expand _SSH_
  3. Expand _Tunnels_
  4. In _Source Port_, enter the port you want your local system to use, 8080
  5. In _Destination_, enter the remote system and port, remotehost:80
  6. Click _Add_

Enjoy.