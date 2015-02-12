---
layout: post
permalink: /fix-adb-server-didnt-ack
title: "Fix: ADB server didn't ACK"
category: ["adt", "android-coding", "dev-tools"]
tags: 
---
## The Infamous Server Didn't ACK

The Android development environment is driven by the ADB daemon. We ran into an issue where the ADB server was failing to start. If you have run into this as well, follow the steps below to get it fixed!

## The Error

[bash] adb start-server \* daemon not running. starting it now on port 5037 \* ADB server didn't ACK \* failed to start daemon \* [/bash]

## Debugging

The following command will essentially tell you why the ADB daemon is failing to initialize: [bash] adb nodaemon server [/bash]

## The Fix(es)

- **Invalid content in adb\_usb.ini. Quitting.**
- Delete the contents within your `adb\_usb.ini` file

- **Unable to bind to tcp:5037**
- Kill the existing ADB process in memory
