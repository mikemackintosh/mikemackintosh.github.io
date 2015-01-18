---
layout: post
title: Scarecrow - A RESTful Interface For Spamhaus Feeds
tags: 
---

Scarecrow is the easiest way to consume the Spamhaus feeds which is traditionally powered by DNS. Although effective in SMTP services, I've found much benefit in using it to protect web apps from malicious and fraudulent behavior. Although it's simple to query DNS is most common web languages, it comes with headaches, buffer and cache issues, as well as control of which servers you actually hit.

Scarecrow allows you to submit a GET request with the route being your IP (or malware domain) in question, and you receive back a message, a code, and an array of results. This response is JSON encoded and can be easily decoded into an array, hash or object.

See this project on [Github: mikemackintosh/spamhaus-scarecrow](https://github.com/mikemackintosh/spamhaus-scarecrow).

## Usage

Simply run it with `rock`:

    rock build
    rock run
    
Or get your dependencies with `bundler` and serve with `shotgun`:

    bundle install
    shotgun --server=thin --port=8000 app.rb

## Running rbldns

Add the following to your `spamhaus-sync.sh` script, if you added the `authbl` and `botnetcc` feeds:

    # Create it into a dns file
    sed -i '2s/^/:127.0.0.6:https:\/\/github.com/mikemackintosh/spamhaus-scarecrow?query=\/$\n/' botnetcc
    sed -i '2s/^/#$TTL 300s\n/' botnetcc
    sed -i '2s/^/#$SOA 5m localhost. hostmaster.localhost. 1501162140 1h 10m 5d 30s\n/' botnetcc

This will add the correct zone file headers needed to successfully receive a response for the `botnetcc` feeds.

Next, you can start your `rbldns` daemon with the following command. This will create a new zone, `any.dnsbl`, which matches several of the more important lists.

    /usr/sbin/rbldnsd -b 0.0.0.0 -f \
        -r /usr/local/rbldns \
        any.dnsbl:ip4set:sbl  \
        any.dnsbl:ip4tset:xbl \
        any.dnsbl:ip4tset:authbl \
        any.dnsbl:ip4tset:botnetcc \
        any.dnsbl:dnset:dbl 

## Consuming

You can easily consume scarecrow with a HTTP GET request:

    scarecrow = JSON.parse HTTParty.get('http://localhost:8000/<ip>').body
    if scarecrow["code"] > 0
        puts "Yes, this is blocked"
    end

### Responses

A response of `-1` or `0` means that there is no malicious match.

  - **Known Spammers** have a code of `2`
  - **Known Botnet Zombies** have a code of `3`
  - **Known Bruteforces** have a code of `4`
  - **Known Malware** have a code of `5`
  - **Known BotnetC&C** have a code of `6`
