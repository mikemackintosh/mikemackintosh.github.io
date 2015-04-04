---
layout: post
title: Maximize Key Strength for Tmate
category: "How To"
tags: tmate pairing ssh keys
redirect_from:
 - /2015/01/26/maximize-key-strength-for-tmate/
---

### Introduction

TMate is a pretty cool tool for sharing shell sessions remotely. I modified the `create_keys` script to read as the following, which maximizes the key strength for the 3 key types:

{% gist mikemackintosh/1f00c7f703317d1fe5e4 %}