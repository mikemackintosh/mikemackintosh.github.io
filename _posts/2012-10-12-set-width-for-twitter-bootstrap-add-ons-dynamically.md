---
layout: post
permalink: /set-width-for-twitter-bootstrap-add-ons-dynamically
title: "Set Width for Twitter Bootstrap Add-On's Dynamically"
category: "jQuery"
tags: add-on bootstrap dynamic forms input jquery organizeaddons twitter coding design jquery-coding twitter-bootstrap
---
### Introduction

In an effort to prioritize and better manage my time, I have invested in using open resources such as [Twitter Bootstrap](http://twitter.github.com/bootstrap/) and alike. One of the features I liked was the form input class `add-on`. The `add-on` allows you prepend or append a legend or key to an input field. It is visually appealing and makes the user experience better.

One bone I had to pick was to align the width of the `add-on` when there was varaible length text inside.

## The Function: organizeAddOns

In an effort to remedy this, I came up with the below jQuery function:

    $.fn.organizeAddOns = function(){ 
        maxwidth = 0; 
        $.each($(this), function(k, v){ 
            if( $(v).width() > maxwidth ){ 
                maxwidth = $(v).width(); 
            } 
        }); 
    
    
        $(this).width(maxwidth); 
    }

## Usage

    $(".add-on").organizeAddOns();

