---
layout: post
permalink: /create-wordpress-like-slug
title: "Create Wordpress/CI Like Slug"
category: "PHP"
tags: ci code-igniter eregi exressions php-2 php5 regular-expressions slug wordpress
---
### Creating a Slug
I was working on a project that used a router to handle the URI requests. It was similar to Code Igniter and Wordpress where I had a need for a slug. The following worked perfectly!

## The Code
```
// $slug = Your Subject To Sluggify 
// $hyphenate = Set true if you wish to NOT turn whitespace into hyphens 
function create_slug($slug, $hyphenate = true){ 
  $slug = strtolower($slug); 

  if($hyphenate){ 
    $slug = preg_replace("/[-\s\W]/","-",$slug); 
  } 

  return preg_replace("/[^a-z0-9-]/", "",strtolower($slug)); 
}

**Note:** Function updated on 2-Feb 2012 to enable space-hyphen auto replace by default

## Examples

Below, you can find a working example:

```
echo create_slug('Here Is An Example); // returns: here-is-an-example 
echo create_slug('ThisIs (Character) !intensive!!', false); // returns: this-ischaracterintensive
```