---
layout: post
title: Ruby String Encoding Weirdness
tags: ruby utf-8 ascii-8bit string encoding
---

This was REALLY stupid. I have a python script that adds a HTTP Header to it's requests. I was using this header to validate the user (think Bearer), but it kept returning `nil`.

{% gist mikemackintosh/b5d8e474156eea2cdcaf example.rb %}

I scratched my head for a few minutes trying to figure out if it was whitespacing or what. Of course, when dealing with strings, you should always check encoding. Come to find out, that's exactly what the problem was.

{% gist mikemackintosh/b5d8e474156eea2cdcaf get_encoding.rb %}

In Ruby, you can change encoding with the `String#encode`, like in the snippet below.

{% gist mikemackintosh/b5d8e474156eea2cdcaf change_encoding.rb %}

Once the encoding was changed, I was able to use the new string the query `ActiveRecord` correctly, and got exactly what I needed.
