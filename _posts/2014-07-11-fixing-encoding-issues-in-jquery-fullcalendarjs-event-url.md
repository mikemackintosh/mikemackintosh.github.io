---
layout: post
permalink: /fixing-encoding-issues-jquery-fullcalendar-js-event-url
title: "Fixing Encoding Issues In jQuery Fullcalendar.js Event URL"
category: "jQuery"
tags: encoding fullcalendar fullcalendar-js html-encoding html-entities jquery json
---

### Introduction

I was using PHP to run `json_encode` on an array to feed to Fullcalendar.js. This worked flawlessley, until I realized one of the attributes, `url` was encoded wrong. It was rendering a WP Admin url with `&amp;` instead of `&`.

## The Fix

There is a callback in the plugin called `eventRender`. This is called during the rendering of the event. To fix the encoding, what we could do is the following:

    eventRender: function(event, element) {
        element.attr('href', $(element).attr('href').replace(/&amp;/g, '&'));
    },

This trick grabs the existing URL `href` and sets the elements new `href` to the fixed url using replace.

Enjoy.

