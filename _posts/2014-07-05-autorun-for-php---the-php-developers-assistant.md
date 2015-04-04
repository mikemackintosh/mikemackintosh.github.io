---
layout: post
permalink: /autorun-for-php
title: "Autorun for PHP - The PHP Developers Assistant"
category: "Project"
tags: assistant autorun cli developer github php python
---
### Introduction

Here is a quick `python` script that listens for file modifications recursively. If a file change is found, the file you specified will execute. The purpose was to alleviate the headache of programming PHP and having to switch contexts between a terminal and your editor to execute your code.

## Usage:

Here are some quick steps to use Autorun. You must have `git` installed and a `python` runtime.

#### Setup the daemon:

    git clone https://github.com/mikemackintosh/autorun.git
    cd autorun
    chmod +x autorun

#### Run the daemon:

`<file>` should be the PHP file you wish to execute, such as `index.php` or a bootstrap file.

If you omit `<path>` then it will use your current directory (`.`). Otherwise, it will listen recursively for file changes in the path you supply.

    ./autorun <file> <path>

## Example:

    $ ./autorun app.php
    Preparing to run: app.php
    
    
    Fatal error: Call to undefined function Rocket\DB\Config() in /somepath/app.php on line 99
    
    
    Execution Completed
    Received a response of: 255
    Took 0.07155585289 seconds


    Success
    
    Execution Completed
    Received a response of: 0
    Took 0.0019392827 seconds

## Conclusion

You can file the whole package on [GitHub](https://github.com/mikemackintosh/autorun).

Enjoy.

