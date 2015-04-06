---
layout: post
permalink: /using-an-ip-for-a-sendmail-relay
title: "Configure a Smart Relay in Sendmail"
category: "How To"
tags: email mail relay sendmail smarthost system-administration
---
### What is Sendmail?

Sendmail is a very simple and effective Mail Transport Agent (MTA) which you can install locally on a server. You can learn more about Sendmail by taking a [look at their website](http://www.sendmail.com/). This is one of the most commonly used MTA's along side [Postfix](http://www.postfix.org/), [Courier](http://www.courier-mta.org/), [Exim](http://www.exim.org/) and others.

Without using an MTA, messages on your local Linux box may be unable to send email. You could still send messages to your local user accounts which would be stored in `/var/mail/<user>`.

## Configuring a Relay

With recent changes in the SMTP spiderweb due to increasing spam, many servers can't openly send to any random email. A lot of times, you need to use a relay or a smarthost in between you and the destination that has permissions to talk to those domains. If you installed Sendmail locally, you may have to point your email to an active external SMTP server as well.

## Configuring Sendmail.cf

All the documentation that is easily found on the internet tells you how to configure a smarthost relay using a FQDN:

You would simply edit `/etc/mail/sendmail.cf`. Within this file, a line starts with `DS`.

To configure your Sendmail install to use a relay by FQDN you would just do:

    DSsome.fully.qualified.domain.com

But if you want to use an IP instead of a FQDN, this information escaped the internet. You will need to wrap your IP address in brackets, like so:

    DS[10.33.99.199]

## Summary

Hopefully this helps you from smashing your head on your desk at 3AM still trying to figure out how to forward to an IP address. It has come up a lot of times with our clients, and they all seem to ask the same question.

