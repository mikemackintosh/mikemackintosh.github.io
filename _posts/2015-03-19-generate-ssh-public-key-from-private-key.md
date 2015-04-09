---
layout: post
title: "Generate SSH Public Key From Private Key"
category: Security
tags: security ssh private-key public-key priv
---
### Introduction

Everyone should know about SSH keys and the benefit they add to securing remote logins. On simple example is enforcing the presence of an SSH private key during the SSH login process, to ensure the identity of a user, assuming the private key is only in possession of the remote user.

## Honey, Have You Seen My Keys?

Sometimes, your private and public keys get lost. If you lose your private key, you need to start from scratch.

If you lose your public key, no fear! You can easily regenerate your public key from a private key using the following command:

```bash
ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub
```

> **Note** Replace `id_rsa` with the key you wish to use