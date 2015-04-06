---
layout: post
permalink: /fix-adb-server-didnt-ack
title: "Fixing ADB server didn't ACK"
category: "Android"
tags: adt android-coding dev-tools
---

### Introduction

The Android development environment is driven by the ADB daemon. We ran into an issue where the ADB server was failing to start. If you have run into this as well, follow the steps below to get it fixed!

    adb start-server 
    \* daemon not running. starting it now on port 5037 
    \* ADB server didn't ACK 
    \* failed to start daemon 
    \* 


## Starting the Server

The following command will essentially tell you why the ADB daemon is failing to initialize: 

    adb nodaemon server

## Results and Fixes

After running this you'll see some of the following reasons. 

  - **Invalid content in adb_usb.ini. Quitting.**
  - Delete the contents within your `adb_usb.ini` file

  - **Unable to bind to tcp:5037**
  - Kill the existing ADB process in memory
