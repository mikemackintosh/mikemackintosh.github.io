---
layout: post
permalink: /no-prompt-with-php-interactive-mode
title: "No Prompt with PHP Interactive Mode"
category: ["uncategorized"]
tags: interactive-mode php-2 php-a php-interactive-mode php5
---
On Debian, install the `libreadline` development package. [bash] sudo apt-get install libreadline6-dev [/bash] Then compile PHP with readline support: [bash] ./configure ... --with-readline=/usr [/bash] And you are all set!

