---
layout: post
title: Log Viewing With Ninetails
category: "Project"
tags: logging data-mining
redirect_from:
 - /system-administration/2015/01/03/log-viewing-with-ninetails/
---

### Introduction
Ninetails is a simple python tool for viewing logs in a very efficient (and colorful) manner. Anyone familiar with intrusion detection, system troubleshooting, or even ops engineers investigating outages, it's important to be able to spot a log anomaly quickly. 

## I Choose You, Ninetails
Take a look:

![Screenshot](https://raw.githubusercontent.com/mikemackintosh/ninetails/master/screenshot.png)

Creating your own colors is as simple as creating a python dictionary. The syntax is as follows:

    colors = {
        'RED':  ['sudo', 'kernel'],
        'ORANGE': ['login'],
        'GREEN':  ['py', 'php', 'launch'],
        'BABYBLUE':  ['discoveryd']
    }

After you download, follow the README. All you'll need to do is `sudo pip install -r requirements.txt`.

## Conclusion

A few thing's I'd like to add include:

  - Community managed filters
  - Self-updating
  - Add more colors

You can check out Ninetails on GitHub here: [https://github.com/mikemackintosh/ninetails](https://github.com/mikemackintosh/ninetails)