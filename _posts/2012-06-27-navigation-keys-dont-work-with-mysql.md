---
layout: post
permalink: /navigation-keys-dont-work-with-mysql
title: "Navigation Keys Don't Work With MySQL"
category: "Gotcha"
tags: mysql with-readline system-administration mysql-cli
---

### Introduction
If you are tired of MySQL throwing out a `~` every time you hit one of your navigation keys such as <kbd>Home</kbd>, <kbd>End</kbd>, and <kbd>Delete</kbd>, look no further.


## Simple Solution
Recompile MySQL with `--with-readline`. The [`readline`](http://cnswww.cns.cwru.edu/php/chet/readline/rltop.html) package is also required to run PHP in interactive mode (`php -a`).

