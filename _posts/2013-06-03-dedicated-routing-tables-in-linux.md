---
layout: post
permalink: /dedicated-interface-routing-tables
title: "Dedicated Routing Tables in Linux"
category: "How To"
tags: aptitude cli debian eth ethernet fabric interface interfaces ip network-2 networking optic reth routing switching ubuntu
redirect_from:
 - /dedicated-interface-routing-tables/
 - /dedicated-interface-routing-tables/feed/
 - /dedicated-interface-routing-tables/trackback/
---
# Linux Routing

Linux is a very powerful platform. It is the framework for thousands of applications and software suites. It's flexibility when it comes to network is second to none, especially for power users. Although other platforms like Windows support [manual routing through multiple gateways](http://windows.microsoft.com/en-us/windows-vista/configuring-multiple-gateways-on-a-network), it does not support [policy based routing](http://en.wikipedia.org/wiki/Policy-based_routing). This is where Linux has the upper-hand.

**Note:** The steps below have been completed and tested on Ubuntu 12.04.2.

# Multiple Interfaces

We recently built and installed a new server which has 4 network interfaces. Two of them are copper/RJ-45 ports and two of them are optical Gigabit Ethernet ports. On this box, if we run an `lspci`, it will show us a hardware profile of the server. Here is a list of our 4 interfaces.

    :~$ lspci -nn | grep Ethernet
    03:03.0 Ethernet controller [0200]: Broadcom Corporation NetXtreme II BCM5706 Gigabit Ethernet [14e4:164a] (rev 02)
    07:01.0 Ethernet controller [0200]: Broadcom Corporation NetXtreme II BCM5706S Gigabit Ethernet [14e4:16aa] (rev 02)
    07:02.0 Ethernet controller [0200]: Broadcom Corporation NetXtreme II BCM5706S Gigabit Ethernet [14e4:16aa] (rev 02)
    07:03.0 Ethernet controller [0200]: Broadcom Corporation NetXtreme II BCM5706 Gigabit Ethernet [14e4:164a] (rev 02)

You can pair up the interfaces by their `[VENDOR:MAKE]` combo's.

To figure out what the name of each interface is, you can take it's PCI location and grep `dmesg`:

    dmesg | grep "07:03.0"

The response would look like:

    [3.852851] bnx2 0000:07:03.0: eth3: Broadcom NetXtreme II BCM5706 1000Base-T (A2) PCI-X 64-bit 100MHz found at mem f6000000, IRQ 19, node addr 00:0a:ba:di:d3:a0

The Gigabit Ethernet looks like this:

    [3.371963] bnx2 0000:07:02.0: eth2: HP NC370F Multifunction Gigabit Server Adapter (A2) PCI-X 64-bit 100MHz found at mem f8000000, IRQ 18, node addr 00:0a:ba:di:d3:a1

Right after it's location there is the interface name. For the two examples above, they are `eth3` and `eth3` respectively. We will need to know these interface names so we can configure them and manage the IP rules. The next step for you would be to configure your `/etc/network/interfaces` file or your `/etc/sysconfig/network-scripts/` depending on your distribution.

Tip! You can always look at the contents of `/etc/udev/rules.d/70-persistent-net.rules` which will show your the name of each interface. You can always rename your interfaces if you want. For example, above `eth0` and `eth3` are alike cards, but divided by the optical ports. We can rename them by changing the interface `NAME`

# Managing the Routing Table

To aid in explanation, here is our interface configuration:

    $ sudo ifconfig -s
    Iface MTU Met RX-OK RX-ERR RX-DRP RX-OVR TX-OK TX-ERR TX-DRP TX-OVR Flg
    eth0 1500 0 2694 0 0 0 6 0 0 0 BMRU
    fab0 1500 0 5517 0 0 0 3224 0 0 0 BMRU
    fab1 1500 0 2682 0 0 0 6 0 0 0 BMRU
    lo 16436 0 256 0 0 0 256 0 0 0 LRU
    mgmt 1500 0 1109 0 0 0 75 0 0 0 BMRU

You can see we named our 4 interfaces.

- 

`mgmt` will be our general management interface. This is the first RJ-45 port and is going to be on a `10.1.32.x` network. (This was originally `eth0`)

- 

`eth0` is the second RJ-45 port and is going to be on the `10.1.101.x` network. (This was originally `eth3`)

- 

`fab0` and `fab1` are out optical Gigabit Ethernet ports and will be on the `10.1.101.x` network as well. (These were originally `eth1` and `eth2`)

Normally, if traffic is received on an interface, it will be routed back out whichever interface has a default route. This can break IP flows, especially if you are navigating across a Firewall or using NAT. Many systems now adays that use Wireless LAN and Ethernet at the same time, have individual default routes which is why you do not see this issue.

To resolve this, you would create an independent routing table for each interface.

**Step 1)** Create a new routing table:

    echo "61 eth0" | sudo tee /etc/iproute2/rt_tables

**Step 2)** Add Default Routes for each Interface:

    ip route add default dev eth0 table eth0

**Step 3)** Add Routing Policy for each Interface:

    $ sudo ip rule add from 10.1.101.61 table eth0

And that's it! Now, when you send traffic to `10.1.101.61` it will be routed by the `eth0` interface instead of the `mgmt` interface! Repeat the above steps for every interface you have.

If you run a `ip route` you should see your updated routing table:

    ~$ sudo ip route
    default dev mgmt scope link
    10.1.32.0/24 dev mgmt proto kernel scope link src 10.1.32.61
    10.1.101.0/24 dev eth0 proto kernel scope link src 10.1.101.61
    10.1.101.0/24 dev fab0 proto kernel scope link src 10.1.101.62
    10.1.101.0/24 dev fab1 proto kernel scope link src 10.1.101.63

# Implementation

The above steps work great, but there can be an issue if you restart the machine. Default route will be assigned to the first up interface, which can round-robin. We can take steps to prevent this from happening which includes editing `/etc/network/interfaces`.

Add the following lines to the end of your `/etc/network/interfaces`:

    # Adds default route for box
    up route add default dev mgmt
    
    
    # Adds default route for eth0 interface
    up route add default dev eth0 table eth0
    up rule add from 10.1.101.61 table eth0

Now when you restart your machine, the above commands will be executed and restore your interface routes and rules.

# Worth Mentioning

There are a few gotcha's that may leave you scratching your head. Here are some tips and tricks to solve them:

**Problem 1:** You receive an interface not configured when using `ifdown`:

    ifdown: interface ethx not configured

The fix would be to use the `ip link` command. You can change the status of the interface by issuing `sudo ip link set ethx down`.

**Problem 2:** Gigabit Fiber interface does not come up

    sudo ethtool -r ethx

This uses the utility known as [ethtool](http://www.linuxcommand.org/man_pages/ethtool8.html), which is a very handy CLI networking tool. The `-r` option tells the interface to restart negotiations. Sometimes on system start, the interface does not do auto-negotiation and needs a little shove.

**Problem 3:** Multiple interfaces within the same subnet are a Virtual Machine Guest

This is a little trickier. Because of the way the interfaces interact with the host machine at a layer-2 level, you will need to apply ARP blocking on the non-desired interfaces. One utility for this would be `arptables`. It works exactly like `iptables` but at Layer 2.

For example, if you do not want interface `fab0` (10.1.101.62) responding to traffic destined to `fab1` (10.1.101.63), you can do:

    sudo arptables -A INPUT -j DROP -i fab0 ! -d 10.1.101.62
    sudo arptables -A INPUT -j DROP -i fab1 ! -d 10.1.101.63

For this to take effect, you must enable ARP filtering at the kernel level:

    echo "net.ipv4.conf.all.arp_filter = 1" >> /etc/sysctl.conf

To view your `arptables` rules, execute:

    ~$ sudo arptables -vnL
    Chain INPUT (policy ACCEPT 988 packets, 27664 bytes)
    -j DROP -i fab0 -o * ! -d 10.1.101.62 , pcnt=43899 -- bcnt=1229K
    -j DROP -i fab1 -o * ! -d 10.1.101.63 , pcnt=44655 -- bcnt=1250K
    
    
    Chain OUTPUT (policy ACCEPT 988 packets, 27664 bytes)
    
    
    Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)

This will prevent the upstream switch from storing the wrong MAC address in it's ARP table and subsequently sending traffic to the correct interface.

