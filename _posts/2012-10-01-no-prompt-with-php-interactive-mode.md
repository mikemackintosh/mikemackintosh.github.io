---
layout: post
permalink: /no-prompt-with-php-interactive-mode
title: "No Prompt with PHP Interactive Mode"
category: system-administration
tags: interactive-mode php php php-cli php-interactive-mode php5
---
On Debian, install the `libreadline` development package. 

    sudo apt-get install libreadline6-dev

Then compile PHP with `readline` support: 

    ./configure ... --with-readline=/usr

And you are all set!

