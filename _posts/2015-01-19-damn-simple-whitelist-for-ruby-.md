---
layout: post
title: "Implementing a Damn Simple Whitelist"
tags: ip network ruby whitelist blacklist
category: "Project"
redirect_from:
 - /networking/2015/01/19/damn-simple-whitelist-for-ruby-/
---

### Introduction"
This class was created to be exactly what it's called, damn simple white-listing. <span class="mute">(And for those captain-obvious' out there, yes, you can make it a black-list too. I'm so proud of you!)</span>

You can pass it whitelists if the form of a file. The default location for the `WHITELIST_FILE` is `/etc/whitelist`, and takes 1 IP range or host per line. It will only parse legit IP's, which are validated using the `IPAddress` gem. 

You can also pass a hard-coded array in `WHITELIST_IPS`. These are good for local addresses that should never be blocked.

**Note:** This script does require a 3rd-party library, `ipaddress`. You can get that installed using `gem install ipaddress`.


## Usage

I don't think it can get much easier than this:

{% gist mikemackintosh/80d4538d67b070878c9f %}

If you want to get all whitelisted IP addresses, you can use the `#all` method. 
