---
layout: post
title: Generate OpenVPN Client Certs with Ruby
tags: ruby openssl openvpn gem ssl
category: "How To"
redirect_from:
 - /system-administration/2015/01/09/generating-openvpn-client-certs-with-ruby/
---

### Introduction
I was working on a project recently that dealt with generating and revoking SSL certs for OpenVPN clients in a simple and automated fashion, without paying for Access Server. Easy-RSA is easy to use, but totally unsecure to automate (think Shellshock..), and in order to replicate certs for backup, I would need to rsync, GlusterFS or some weird file transfer hacks. In all honesty, it would have been easier to save encrypted certs to a database so the end user can easily pull them down when needed.


## Using Ruby

To accomplish this without breaking down to a shell or running some kind of `system()` command, I abused the `openssl` gem. The following code snippet, which has been modified from it's original version, can easily be adapted to fit your needs.

{% gist mikemackintosh/f227fa77b78aad4432f5 openvpn-pki.rb %}


## Conclusion

And to use the above class, you can easily call the class like so:

{% gist mikemackintosh/f227fa77b78aad4432f5 usage.rb %}

> Thanks to [Roman Gaufman](https://disqus.com/by/hackeron/) for cleaning up the code!