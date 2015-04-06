---
layout: post
permalink: /php-ssh2-class-with-exception-handling
title: "PHP SSH2 Class with Exception Handling"
category: "System Administration"
tags: php-ssh php-ssh2 ssh2 ssh2_exec ssh2_shell vt100 linux remote shell prompt
---

### Introduction

In a previous role, I was responsible for running diagnostics on remote networking devices and parsing the output to gather important details on their current status and health. To keep all aspects of the code consistent, which was written in PHP, I used the `SSH2` PECL module, which I was easily able to dynamically load into the PHP config.

The downside is that, there was not much error handling in this default class. Because the code was being used to support a mission critical network, that a lot of people here used on a daily basis, there was no margin for error. 

Below I have included an example and the source class which adds exception handling to `SSH2` for PHP.

## Here's an Example

{% gist mikemackintosh/4a295d21f41d982d996a auth_example.php %}
{% gist mikemackintosh/4a295d21f41d982d996a example.php %}

## Getting the Code

The code is hosted in a Gist, which we have included below:

{% gist mikemackintosh/4a295d21f41d982d996a ssh2.php %}


