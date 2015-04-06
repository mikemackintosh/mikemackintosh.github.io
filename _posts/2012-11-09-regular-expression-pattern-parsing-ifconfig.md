---
layout: post
permalink: /regex-pattern-parsing-ifconfig
title: "Regular Expression Pattern Parsing ifconfig"
category: "System Administration"
tags: ifconfig interfaces ip-address mac-address networking parsing php-2 regex
---

This following `gist` allows you to easily parse the output of `ifconfig`.

{% gist mikemackintosh/3826c763bb3d247ffe36 ifconfig.php %}

**Note:** This regex is designed to skip loopback by default.