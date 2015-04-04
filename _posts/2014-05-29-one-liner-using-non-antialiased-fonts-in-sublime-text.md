---
layout: post
permalink: /one-liner-using-non-antialiased-fonts-sublime-text
title: "Using Non Antialiased Fonts In Sublime Text "
category: "One-Liner"
tags: sublime sublime-text ide editor
---
## Sublime Text Antialiasing

I use `vim` for most of my editing needs. Once in the while when I need a GUI editor, I use [Sublime Text 3](http://www.sublimetext.com/3). One of the things that bothers me the most when using Sublime Text vs `vim` is the antialiasing of font's. They look crazy dumb clunky and bulky.

If you want to disable Antialiasing for Sublime Text 2/3 fonts, add the following to your `Settings - User` preferences file.

    "font_options": ["no_antialias"]

