---
layout: post
permalink: /php-fastcgi-post-requests-failing
title: "PHP FastCGI POST Requests Failing"
category: "System Administration"
tags: 4gh cannot-post-php fast-cgi fastcgi godaddy godaddy-4gh mod_rewrite php php-post-not-working post-not-working wordpress
redirect_from:
 - /php-fastcgi-post-requests/
 - /php-fastcgi-post-requests/trackback/
---

### Introduction
I was working with a site that had to be migrated from a Virtual Dedicated Server to a GoDaddy 4GH plan and ran into an issue where POST forms passed through `mod_rewrite` were not being populated to `$_POST`. I realized that in a FastCGI environment the requests will be redirected as a `GET` string, meaning that your POST data will be lost.

It was a 1-liner in the `.htaccess` file.

## Adding a Fix
If you have a `R=301` in the brackets `[]`, remove it so it looks like below:

    # On FastCGI replace the last line above with this: 
    RewriteRule ^(.\*)$ index\.php?/$1 [L] 

Enjoy.