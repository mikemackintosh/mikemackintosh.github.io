---
layout: post
title: "Attaching to Existing Processes in *nix"
category: system-administration
tags: linux unix pid terminal console
---

# Background
I was working on a project where we had long-lived console sessions open with root terminals available. If you don't know what that means, simple put: **THAT IS ZOMG BAD, SO EFFING BAD!**.

This means that if someone walked up to one of the servers, or had access to a tool like Foreman, KVM, or a console emulator, they would have root access to your machine. In a production environment, this should never happen.

# The Code

{% gist mikemackintosh/e657c44a82b10ea49d3e w_command_output.sh %}


{% gist mikemackintosh/e657c44a82b10ea49d3e monitor.sh %}


{% gist mikemackintosh/e657c44a82b10ea49d3e get_pid.sh %}

