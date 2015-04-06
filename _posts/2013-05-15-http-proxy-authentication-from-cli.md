---
layout: post
permalink: /http-proxy-authentication-from-cli
title: "HTTP Proxy Authentication From CLI"
category: "How To"
tags: authenticate headless http proxy secure servers www-authenticate
---
# HTTP Proxies

We deploy a firewall between our internet zones and working zones. This helps segregate possible flawed traffic from testing and keeps possibly harmful traffic from affecting the internet. To access the network from our workstations, we have to authentication through a firewall proxy for port 80 and 443. This box will keep track of our source IP and it's authentication status. We use timeout rules to prevent wrongfully accessed machines from getting out using any protocol until authenticated.

# Servers Don't Have Browsers?!

Normally, servers deployed in a data center style infrastructure run headless. This means there is no desktop, X-Server, Gnome, or KDE running. Everything you do is via CLI.

The normal process to authenticate to the proxy is to open a browser and access a page that crosses this firewall. The firewall will ask you to log in using WWW-Authenticate. If you don't have a browser, how could you do this?

# Using WGET

[Wget](http://www.gnu.org/software/wget/) is a utility, very similar to [cURL](http://curl.haxx.se/), which perform HTTP GET's by default. Luckily, it has many, many options configurable. Two options will solve our problem include `user` and `password`.

These two options will pass a username and password to the WWW-Authenticate headers. This is exactly what we need.

    wget http://google.com --user=fwuser--password=fwpassword

This will display the following output:

    --2013-05-15 13:42:51-- http://google.com/
    Resolving google.com (google.com)... 74.125.226.201, 74.125.226.206, 74.125.226.192, ...
    Connecting to google.com (google.com)|74.125.226.201|:80... connected.
    HTTP request sent, awaiting response... 401 Unauthorized
    Failed writing HTTP request: Bad file descriptor.
    Retrying.
    
    
    --2013-05-15 13:42:52-- (try: 2) http://google.com/
    Connecting to google.com (google.com)|74.125.226.201|:80... connected.
    HTTP request sent, awaiting response... 302 Object Moved
    Location: http://google.com/ [following]
    --2013-05-15 13:42:52-- http://google.com/
    Connecting to google.com (google.com)|74.125.226.201|:80... connected.
    HTTP request sent, awaiting response... 301 Moved Permanently
    Location: http://www.google.com/ [following]
    --2013-05-15 13:42:52-- http://www.google.com/
    Resolving www.google.com (www.google.com)... 74.125.26.99, 74.125.26.103, 74.125.26.104, ...
    Connecting to www.google.com (www.google.com)|74.125.26.99|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: unspecified [text/html]
    Saving to: `index.html'
    
    
        [<=>] 10,696 --.-K/s in 0s
    
    
    2013-05-15 13:42:52 (134 MB/s) - `index.html' saved [10696]

Once completed, you can access the internet from your server without the need of a GUI.

