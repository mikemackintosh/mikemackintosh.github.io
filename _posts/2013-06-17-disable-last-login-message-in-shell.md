---
layout: post
permalink: /disable-last-login-message-in-shell
title: "Disable Last Login Message in Shell"
category: "How To"
tags: bash cli command-line last-login sh shell tty vtty
---
## Default Messages When Launching Shell

Depending on how you access your shell, you can be greeted with a variety of messages. These can range from a quick 15-minute snapshot such as [Ubuntu's Landscape message of the day](http://ubuntuforums.org/showthread.php?t=1077244), a quick reference to your TTY and even the last time you logged in and from what virtual terminal or IP it was done on.

To most people, this takes up rows or holds no value. For example, here is a new tab with a `Last Login` message:

![Screen Shot 2013-06-18 at 10.56.17 AM](http://www.highonphp.com/v3/wp-content/uploads/2013/06/Screen-Shot-2013-06-18-at-10.56.17-AM.png)

## Hiding the Last Login

This change is so simple, you might poke yourself in the eyes making sure you are reading it correctly:

    touch .hushlogin

This command creates a blank file named `.hushlogin` and if it is detected by bash, it will not display last login information.

Here is an example with it removed:

![Screen Shot 2013-06-18 at 10.56.35 AM](http://www.highonphp.com/v3/wp-content/uploads/2013/06/Screen-Shot-2013-06-18-at-10.56.35-AM.png)

