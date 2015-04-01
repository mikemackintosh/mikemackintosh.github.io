---
layout: post
title: "Generate SSH Public Key From Private Key"
category: Security
tags: security ssh private-key public-key priv
---

```shell
ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub
```