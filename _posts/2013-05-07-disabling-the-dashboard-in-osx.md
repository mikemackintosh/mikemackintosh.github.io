---
layout: post
permalink: /disabling-the-dashboard-in-osx
title: "Disabling the Dashboard in OSX"
category: "OS X"
tags: dashboard disable leopard lion mac mountain-lion osx snow-leopard
---
### The OSX Dashboard

One of the features added into OSX many years back include the [Dashboard](http://support.apple.com/kb/ht2492). The dashboard can be accessed by either using <kbd>F12</kbd> or setting up a finger gesture. Personally, I never found use out of the dashboard as it required an extra step to access the data, effectively destroying any workflow.

## To Disable

Run the following commands in your Terminal to disable the Dashboard:

    defaults write com.apple.dashboard mcx-disabled -boolean YES
    killall Dock

## To Enable

To enable the Dashboard, execute the following commands:

    defaults write com.apple.dashboard mcx-disabled -boolean NO
    killall Dock

