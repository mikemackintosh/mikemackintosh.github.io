---
layout: post
title: "Installing Package Dependencies in your Cookbook"
category: Chef
tags: chef system-administration sysadm config configuration-management automation apt yum rpm
---

### Introduction

I do love me some configuration management. One of the things that simplifies operational security is the ability to distribute and enforce configuration across a massive amount of nodes.

Unspecific to security, sometimes you need to check if a package is installed, and if it's down, download and install it. Unfortunately, there is no simplified workflow to get this done.

## Updating the Recipe

The following gist ensures that you download and install the package as well updating the apt cache. Notice though, this is for `deb` files which do not exist in a mainstream repo.

{% gist mikemackintosh/45fb3251c71333601c70 gistfile1.rb %}

## Conclusion

You can obviously modify this in many ways to make it useful how you need. 