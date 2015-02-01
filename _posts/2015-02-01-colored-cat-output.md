---
layout: post
title: Want Colored `cat` Output?
tags: config-management output cat source-code
---

If you read a lot of source code or configuration files, and you're a loser that doesn't use `vim`, then these two commands will come in handy for you:

    sudo easy_install Pygments
    alias cat='pygmentize -O console256 -g'

It will make your stupid output look pretty. 