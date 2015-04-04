---
layout: post
title: String Encoding Weirdness
tags: ruby utf-8 ascii-8bit string encoding
category: "Ruby"
redirect_from:
 - /2015/02/06/ruby-string-encoding-weirdness/
 - /web-apps/2015/02/06/ruby-string-encoding-weirdness/
---

### Introduction
I was investing some more time into rebuilding Cronus for TitanOSX. Within the communications from the titan client to Cronus, I have python make an HTTP call that adds a HTTP Header to it's requests. I was using this header to validate the user (think Bearer) within an ActiveRecord call, but it kept returning `nil`.

## Example

{% gist mikemackintosh/b5d8e474156eea2cdcaf example.rb %}

I scratched my head for a few minutes trying to figure out if it was white-spacing or not, so instinctively I tried things like `#strip` to no avail. Of course, when dealing with strings, you should always check encoding. Come to find out, that's exactly what the problem was.

{% gist mikemackintosh/b5d8e474156eea2cdcaf get_encoding.rb %}

## Resolving the Issue

In Ruby, you can change encoding with the `String#encode`, like in the snippet below.

{% gist mikemackintosh/b5d8e474156eea2cdcaf change_encoding.rb %}

Once the encoding was changed, I was able to use the new string the query `ActiveRecord` correctly, and got exactly what I needed.
