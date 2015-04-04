---
layout: post
title: "Attach to Existing Processes in *nix"
category: "How To"
tags: linux unix pid terminal console
redirect_from:
 - /system-administration/2015/02/09/attaching-to-existing-processes/
---

### Introduction
I was working on a project where we had long-lived console sessions open with root terminals available. If you don't know what that means, simple put: **THAT IS ZOMG BAD, SO EFFING BAD!**.

This means that if someone walked up to one of the servers, or had access to a tool like Foreman, KVM, or a console emulator, they would have root access to your machine. In a production environment, this should never happen.


## Checking Your Sessions

First, let's see what sessions are out there that you may need to attach to:

{% gist mikemackintosh/e657c44a82b10ea49d3e w_command_output.sh %}

From the example, you can see that the `root` user had the console open since `31Jan15`. That's a long time. 

You can use the following snippet to grab the `$PID`: 

{% gist mikemackintosh/e657c44a82b10ea49d3e get_pid.sh %}


## Monitoring The Output

Depending on your OS, you should have `strace` available. Using `strace` we can attach to an existing PID and if we pass `read,write` permissions, we can interact with it as well:

{% gist mikemackintosh/e657c44a82b10ea49d3e monitor.sh %}

I also like the `script` command. Passing in a device descriptor, you can open the shell and check it's history:

{% gist mikemackintosh/e657c44a82b10ea49d3e using_script.sh %}

**Note:** Some systems don't support for the `-f` flag, like OS X.