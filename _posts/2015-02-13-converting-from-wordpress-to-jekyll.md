---
layout: post
title: "Convert From Wordpress to Jekyll"
category: ["How To"]
tags: wordpress jekyll convert xml export nokogiri ruby
redirect_from:
 - /state-of/2015/02/13/converting-from-wordpress-to-jekyll/
---

### Introduction

Today, I migrated all the posts from `highonphp.com` to here. Since Jekyll is all about the static files, I had to get a way to export the posts from Wordpress, and convert them to flat files.

The exporting was the easy part. All you have to do in Wordpress is click the `Tools -> Export` link, and choose `All Posts`. When you click `Export`, your browser will download an `.xml` file. This file has pretty simple schema which can be used to import it into another Wordpress instance, or in our case, to Jekyll.

## Starting the Migration

To accomplish this, I wanted to make a really simple Ruby script, and while at that, I wanted to play around with Nokogiri some more. Here are the gem's needed for the conversion to work smoothly.

{% gist mikemackintosh/cbef8873fa3b25ed9028 Gemfile %}

The following script is very simple. You pass it a command line argument, the xml file name, and let it run. 

{% gist mikemackintosh/cbef8873fa3b25ed9028 convert.rb %}

Each Wordpress post is stored in a `<item />` schema block. Within the item blocks, all the post attributes can be found, such as tags, categories, status, content, etc. We assign the important ones to a hash, and use the hash to generate the post file.

For the post content, I used a Wordpress Markdown plugin for writing all my posts. Unfortunately on the back-end, this was converted back to HTML. To get it back into Markdown, I used `Reverse_Markdown` which did a great job.

To generate the filenames, the script will pull the publish date of the post, and convert it to a Jekyll timestamp (`YYYY-MM-DD`), and then create a slug from the title. When you also take into account we have the current post status (publish, draft, etc), we can easily drop the new Jekyll post into either `_posts` or `_drafts`. 

## Permalinking and Nginx

One last thing to note is the permalinking. I wanted to make sure all of the `http://www.highonphp.com/links` directed to `http://www.mikemackintosh.com/links` without an issue. Wordpress included a permalink in the export which we can then toss into the Front-Matter of the Jekyll post, to keep the pages uniform. From the server side, I had to redirect the old domain to the new one, with the URI query string intact. You can easily do that with nginx:

{% gist mikemackintosh/cbef8873fa3b25ed9028 nginx.conf %}

Instead of installing a plugin in Wordpress, we get to convert the posts on our own terms.