---
layout: post
permalink: /php-fastcgi-post-requests-failing
title: "PHP FastCGI POST Requests Failing"
category: ["uncategorized"]
tags: 4gh cannot-post-php fast-cgi fastcgi godaddy godaddy-4gh mod_rewrite php-2 php-post-not-working post-not-working wordpress
---
# The FastCGI Issue
I was working with a site that had to be migrated from a Virtual Dedicated Server to a GoDaddy 4GH plan and ran into an issue where POST forms passed through mod\_rewrite were not being populated to $\_POST. I realized that in a FastCGI environment the requests will be redirected as a GET string, meaning that your POST data will be lost. It was a 1-liner in the '.htaccess' file.
# The Fix
If you have a R=301 in the brackets [], remove it so it looks like below: [bash] # On FastCGI replace the last line above with this: RewriteRule ^(.\*)$ index\.php?/$1 [L] [/bash] Enjoy.