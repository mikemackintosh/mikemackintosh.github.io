---
layout: post
title: Scarecrow - A RESTful Interface For Spamhaus Feeds
tags: dns spamhaus brute-force dictionary-attack spam rest ruby
category: "Project"
redirect_from:
 - /web-apps/2015/01/14/scarecrow-a-restful-interface-for-spamhaus-feeds/
---

### Introduction

Scarecrow is the easiest way to consume the Spamhaus feeds which is traditionally powered by DNS. Although effective in SMTP services, I've found much benefit in using it to protect web apps from malicious and fraudulent behavior. Although it's simple to query DNS is most common web languages, it comes with headaches, buffer and cache issues, as well as control of which servers you actually hit.

Scarecrow allows you to submit a GET request with the route being your IP (or malware domain) in question, and you receive back a message, a code, and an array of results. This response is JSON encoded and can be easily decoded into an array, hash or object.

See this project on [Github: mikemackintosh/spamhaus-scarecrow](https://github.com/mikemackintosh/spamhaus-scarecrow).


## Usage

Simply run it with `rock`:

{% gist mikemackintosh/14d2ddbbe94d66926e1f rock_run.sh %}
    
Or get your dependencies with `bundler` and serve with `shotgun`:

{% gist mikemackintosh/14d2ddbbe94d66926e1f manual.sh %}

## Running rbldns

Add the following to your `spamhaus-sync.sh` script, if you added the `authbl` and `botnetcc` feeds:

{% gist mikemackintosh/14d2ddbbe94d66926e1f rewrite.sh %}

This will add the correct zone file headers needed to successfully receive a response for the `botnetcc` feeds.

Next, you can start your `rbldns` daemon with the following command. This will create a new zone, `any.dnsbl`, which matches several of the more important lists.

{% gist mikemackintosh/14d2ddbbe94d66926e1f run.sh %}

## Consuming

You can easily consume scarecrow with a HTTP GET request:

{% gist mikemackintosh/14d2ddbbe94d66926e1f example.rb%}

## Responses

A response of `-1` or `0` means that there is no malicious match.

  - **Known Spammers** have a code of `2`
  - **Known Botnet Zombies** have a code of `3`
  - **Known Bruteforces** have a code of `4`
  - **Known Malware** have a code of `5`
  - **Known BotnetC&C** have a code of `6`
