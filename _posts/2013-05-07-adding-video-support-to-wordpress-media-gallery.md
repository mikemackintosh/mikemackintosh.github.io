---
layout: post
permalink: /adding-video-support-to-wordpress-media-gallery
title: "Add Video Support to Wordpress Media Gallery"
category: "How To"
tags: gallery pictures videos wordpress wp
---
### Introduction

[Wordpress](http://www.wordpress.com) comes out-of-the-box with hundreds of really well implemented features. It's one of the reasons it is the leading blogging software used today. One of the features we will focus on is the built in Wordpress Gallery.

To add a gallery to a post, you can follow the below steps:

**Step 1:** Click Add Media

![Click Add Media](http://www.highonphp.com/v3/wp-content/uploads/2013/05/Screen-Shot-2013-05-07-at-11.19.25-AM-300x210.png)

**Step 2:** Click Create Gallery and select your photos

![Create Gallery](http://www.highonphp.com/v3/wp-content/uploads/2013/05/Screen-Shot-2013-05-07-at-11.19.36-AM-300x200.png)

**Step 3:** Edit and Insert

![Insert Into Post](http://www.highonphp.com/v3/wp-content/uploads/2013/05/Screen-Shot-2013-05-07-at-11.19.54-AM-300x94.png)

If you have noticed though, you can only add Images. If you have uploaded videos to your Media, you cannot add them to a gallery.

## The Fix:

The change we need to make is located within the function `wp_ajax_query_attachments()`. This function is located within the file, `wp-admin/includes/ajax-actions.php`. You can do a search for this function, or look at or around line `1835`.

**_Note_** : The only way to easily support videos within a gallery through Wordpress's default media gallery is to edit a core file. That means that after an upgrade, you may lose this change.

Specifically, change line `1841`:

    's', 'order', 'orderby', 'posts_per_page', 'paged', 'post_mime_type',

To:

    's', 'order', 'orderby', 'posts_per_page', 'paged',

Removing the `'post_mime_type'` array index from this query will not add a constraint your media library to `image/%` mime types.

## Summary

If you go back to a post, you can add or edit a gallery and now videos will display. By default, thumbnails are not created for your videos which you upload. I would suggest the [Video Embed and Thumbnail Generator](http://www.kylegilman.net/2011/01/18/video-embed-thumbnail-generator-wordpress-plugin/) plugin to handle this.

