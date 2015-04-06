---
layout: post
permalink: /gearman-failed-to-set-exception-option
title: "Gearman: Failed to set exception option"
category: Gotcha
tags: client gearman php threading worker
---

If you are working with the Gearman module and receive the following when trying to start the worker daemon: "Failed to set exception option", make sure you install the Gearman packages for your OS. 

    sudo apt-get install gearman

