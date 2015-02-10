---
layout: post
title: "Attaching to Existing Processes in *nix"
category: system-administration
tags: linux unix pid terminal console
---


LONG_LIVED_PID=$(ps aux | \
grep `w | grep -E "\s(tty.*?|console.*?)\s" |awk '{print $2}'` | awk '{print $2}' | grep -v "grep")
 
sudo strace -p$LONG_LIVED_PID -s9999 -e write,read