---
layout: post
permalink: /bing-and-phantom-bandwidth
title: "Bing and Phantom Bandwidth"
category: "Security"
tags: bing bots crawl crawl-bots google robots robots-txt site5
---
### Bing is the Devil

We have a client that is hosted on [Site5](http://www.site5.com/in.php?id=22285). They are a great host and with Fantastico, installing and updating Wordpress is amazingly simple.

Recently, I have been getting emails from this client's account stating that the bandwidth has been exhausted. I thought to myself, "How could this be? Their account is suspended for lack of payment?!" Their website points to a suspended page with a `.htaccess` redirecting all traffic to `suspended.php`.

## The Proof

Take a look at this screenshot:

![AWStats Screenshot](http://www.highonphp.com/v3/wp-content/uploads/2013/04/attacked_by_bing-e1365445411461.jpg)

If you look at the viewed traffic, only `43.79 MB`'s of traffic was transferred. On the reverse side, `14.34 GB`'s of unviewed traffic were passed. What caused this unviewed traffic?

I took to the `access_logs` to figure out what this traffic was. This is what we found:

    87.118.126.156 - - [08/Apr/2013:07:19:51 -0500] "POST /wp-login.php HTTP/1.1" 301 301 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:51 -0500] "GET /suspended.php HTTP/1.1" 200 521813 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:53 -0500] "GET /wp-admin/ HTTP/1.1" 301 301 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:53 -0500] "GET /suspended.php HTTP/1.1" 200 521813 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:54 -0500] "POST /wp-login.php HTTP/1.1" 301 301 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:54 -0500] "GET /suspended.php HTTP/1.1" 200 521813 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:56 -0500] "GET /wp-admin/ HTTP/1.1" 301 301 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:56 -0500] "GET /suspended.php HTTP/1.1" 200 521813 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:57 -0500] "POST /wp-login.php HTTP/1.1" 301 301 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:57 -0500] "GET /suspended.php HTTP/1.1" 200 521813 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:59 -0500] "GET /wp-admin/ HTTP/1.1" 301 301 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"
    87.118.126.156 - - [08/Apr/2013:07:19:59 -0500] "GET /suspended.php HTTP/1.1" 200 521813 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)"

As you can see above, Bing kept crawling out page but because of the 301 Redirect it got caught in a loop. It's interesting that only Bing experienced the issue, and not Yahoo, Google or any of the hundreds of other crawlers.

## The Fix:

I made a change to the `robots.txt` file. The below configuration allows all bots but MSN to crawl your site.

    User-agent: *
    Disallow:
    
    
    User-agent: MSNbot
    Disallow: /

We saw an almost complete drop in bandwidth utilization after the change was implemented.

I still don't understand how Bing is a search engine, and I really wonder why they still crawl, who even uses it anymore?!

