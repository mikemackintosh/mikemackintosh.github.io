---
layout: post
title: "Add Comments to JunOS XML"
category: "How To"
tags: network automation sdn junos xml
redirect_from:
 - /network-automation/2015/02/11/adding-commets-to-junos-xml/
---

### Introduction
One of the projects I have been working on makes a NETCONF call from a worker script to several JunOS devices to apply changes to a blacklist/whitelist prefix-list. This serves essentially as a `rapidresponse` ACL, so we need the ability to track changes, etc.

While working on the project, I wanted to be able to annotate the config changes with comments. Generically, in JunOS, you can add `/* */` comment strings using the `annotate` command or in set form.

## JUNOS XML Schema
When working with XML, it is a little different. You can utilize the `<junos:comment></junos:comment>` block, and precede the element you wish to comment.

{% gist mikemackintosh/497f5e11fb3388904c3d prefixlist.xml %}

**Note**: One thing to remember is that you must include the `/* */` inside of your `<comment></comment>` block.