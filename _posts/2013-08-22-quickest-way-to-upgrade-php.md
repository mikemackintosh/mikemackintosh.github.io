---
layout: post
permalink: /upgrading-php
title: "Quickest Way To Upgrade PHP"
category: "How To"
tags: configure config php php-config upgrade
---
### Introduction

One of the most common reasons for security breaches is out of date software. This could mean applications like Wordpress or even the parser and HTTP server behind it like PHP and nginx. Personally, I feel there really is no excuse to let minor and even major releases roll by other than pure laziness or lack of dedication to the project. However, there are times when you start the upgrade process and you either lost your original `./configure` string or didn't realize that your configure options were in `php -i` or `<? phpinfo(); ?>`.

Regardless of which category you fall into, this trick will make it easier than pie to manage the upgrade process. All you need to do is download the tarball from [php.net](http://www.php.net) and extract it.

One of my co-workers lost their original `./configure` string and he was enlightened by the tip below.

## The One-liner

You can reference your current PHP configuration options by accessing the `php-config` binary located within `PHP_ROOT/bin`.

    ./configure `php-config --configure-options`

No need to hunt down your bash history, your notes or even `php -i`.

