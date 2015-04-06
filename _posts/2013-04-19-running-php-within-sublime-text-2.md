---
layout: post
permalink: /building-php-within-sublime-text-2
title: "Run PHP Within Sublime Text 2"
category: "How To"
tags: build editor ide notepad php-2 run text-editor
---
### What is Sublime Text?

The [Sublime Text](http://www.sublimetext.com/) family of software had provided some real innovation when it comes to text editing. Their extremely lightweight and minimalistic editor can be extended far beyond imagination and really give thousand dollar software competition.

One of the things we do is code small PHP snippets. When it comes to projects, we prefer Zend Studio. For small pieces of code, Sublime Text 2 was exactly what we wanted. It was lacking one thing though; the ability to run PHP in-app.

Sublime provide functionality known as Build. This allows you to execute the open file through a builder, such as `make`, and you can even define your own. That is exactly what we did and turned this $70 software package into an irreplaceable tool.

## Creating a PHP Builder

To create a PHP build system, go to <kbd>Tools</kbd> -> <kbd>Build System</kbd> -> <kbd>New Build System</kbd>.

Add the following contents to this new file, and save it as `PHP.sublime-build`:

    {
    "cmd": ["/usr/bin/php", "-f", "$file"]
    }

**Note** : Each argument needs to be passed as a separate item in the array.

## Using Your New Builder

Go back to your PHP file, and press <kbd>Cmd</kbd>+<kbd>B</kbd>. This will build your code. The output of the command will appear in the console if you have it open.

## Summary

Adding simple functionality like the above, really adds weight to such a lightweight editor. All of the customizations can really give other mainstream editors like Eclipse a run for their money!

