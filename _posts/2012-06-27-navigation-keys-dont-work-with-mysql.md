---
layout: post
permalink: /navigation-keys-dont-work-with-mysql
title: "Navigation Keys Don't Work With MySQL"
category: ["uncategorized"]
tags: mysql-2 with-readline
---
If you are tired of MySQL throwing out a `~` every time you hit one of your navigation keys such as <kbd>Home</kbd>, <kbd>End</kbd>, and <kbd>Delete</kbd>.

Recompile MySQL with `--with-readline`. The [`readline`](http://cnswww.cns.cwru.edu/php/chet/readline/rltop.html) package is also required to run PHP in interactive mode (`php -a`).

