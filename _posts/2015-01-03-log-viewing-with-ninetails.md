---
layout: post
title: Log Viewing With Ninetails
category: system-administration
tags: logging data-mining
---

![ninetails](https://www.dropbox.com/s/fwi2oplluoxbblu/Screenshot%202014-07-26%2014.24.38.png?dl=1)

Ninetails is a simple python tool for viewing logs in a very efficient (and colorful) manner. Anyone familiar with intrusion detection, system troubleshooting, or even ops engineers investigating outages, it's important to be able to spot a log anomaly quickly. 

Take a look:

![Screenshot](https://www.dropbox.com/s/0c3ze12ovjpttnp/Screenshot%202014-10-27%2010.11.49.png?dl=1)

Creating your own colors is as simple as creating a python dictionary. The syntax is as follows:

    colors = {
        'RED':  ['sudo', 'kernel'],
        'ORANGE': ['login'],
        'GREEN':  ['py', 'php', 'launch'],
        'BABYBLUE':  ['discoveryd']
    }

After you download, follow the README. All you'll need to do is `sudo pip install -r requirements.txt`.

#### To-Do's:

A few thing's I'd like to add include:

  - Community managed filters
  - Self-updating
  - Add more colors

You can check out Ninetails on GitHub here: [https://github.com/mikemackintosh/ninetails](https://github.com/mikemackintosh/ninetails)