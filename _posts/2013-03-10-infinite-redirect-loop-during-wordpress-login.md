---
layout: post
permalink: /wordpress-admin-redirect-loop
title: "Infinite Redirect Loop during Wordpress Login"
category: "Gotcha"
tags: admin apache-2 htaccess nginx php-2 rewrite rewrite-rule wordpress
---
### The _Other_ Loop

[WordPress](http://www.wordpress.com) has a very intuitive and straight forward admin interface. This is one of the reasons I choose to deploy client projects on WordPress. It's abilities have made it one of the leading software packages for content aggregation. Although it's almost perfect, there are still some things that users and administrators may over look which could lead you to experience a bug. A infinite redirect loop when logging in could be one of them. Luckily for them, I get to work out all the bugs before the keys are handed over.

## The Fix

This fix is pretty easy. What happens depending on your configured settings and what URL (including subdomain) you access the login page from, you confuse WordPress. WordPress knows it needs to authenticate the user, but if this is happening after the login form already posted, you get stuck in an infinit loop.

## For Apache's `.htaccess`

Add the following just below the `RewriteBase` directive in your sites `.htaccess`:

    RewriteRule ^wp-admin - [L]

