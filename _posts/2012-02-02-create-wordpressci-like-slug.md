---
layout: post
permalink: /create-wordpress-like-slug
title: "Create Wordpress/CI Like Slug"
category: ["uncategorized"]
tags: ci code-igniter eregi exressions php-2 php5 regular-expressions slug wordpress
---
# Creating a Slug
I was working on a project that used a router to handle the URI requests. It was similar to Code Igniter and Wordpress where I had a need for a slug. The following worked perfectly!
# The Code
[php] // $slug = Your Subject To Sluggify // $hyphenate = Set true if you wish to NOT turn whitespace into hyphens function create\_slug($slug, $hyphenate = true){ $slug = strtolower($slug); if($hyphenate){ $slug = preg\_replace("/[-\s\W]/","-",$slug); } return preg\_replace("/[^a-z0-9-]/", "",strtolower($slug)); } [/php] **Note:** _Function updated on 2-Feb 2012 to enable space-hyphen auto replace by default
# Examples
[php] echo create\_slug('Here Is An Example); // returns: here-is-an-example echo create\_slug('ThisIs (Character) !intensive!!', false); // returns: this-ischaracterintensive [/php] Enjoy!_