---
layout: post
permalink: /jquery-wordwrap-function
title: "WordWrap Function"
category: "jQuery"
tags: extended function jquery-2 jquery-wordwrap word-wrap wordwrap wordwrap-toggle
---
### Introduction

I was working on a project where I wanted to WordWrap some content and either display an ellipse or add a toggle button to show/hide more. Looking around I could not find anything that did what I wanted, so I tossed together the below. Performance is great and works pretty good.

## The Code

This snippet is also hosted as a Gist on Github:

{% gist mikemackintosh/57a054de3b0c742122d8 jquery.wordwrap.js %}

## Usage examples



{% gist mikemackintosh/57a054de3b0c742122d8 examples.js %}

## Demo

<div class="wordwrap">
This is a demo of the wordwrap.

### Demo Code

    <div class="wordwrap">
    This is a demo of the wordwrap.
    </div>

    <script>
    $(document).ready(function(){
    jQuery.fn.wordwrap = function(){ 
        
        // Get Args
        var args = arguments[0] || {}; 
        
        // Default Options
        var options = { 
            'type': 'slide', 
            'length': 50, 
            'more_text': 'Show More', 
            'hide_text': 'Hide More', 
            'ellipsis': '...' 
        } 
     
        // Merge defaults with args
        options = $.extend({}, options, args);
     
        $.each($(this), function(key,value){
            var str = $(value).html();
            if(str.length > options.length){
                var display = str.substr(0, options.length-3);
                var displayFill = str;
                $(value).html(display).append(options.ellipsis);
     
                if(options.type == 'slide'){
                    $(value).append('<br /><a class="wordwrap wordwrap-it-'+key+'" href="#">'+options.more_text+'</a>');
                    $(document).delegate('a.wordwrap-it-'+key, 'click', function(e){
                        e.preventDefault();
                        if($(value).find('a.wordwrap-it-'+key).text() == options.more_text){
                            $(value).html(displayFill).append('<br /><a class="wordwrap wordwrap-it-'+key+'" href="#">'+options.hide_text+'</a>');
                            return;
                        }
     
                         $(value).html(display).append(options.ellipsis).append('<br /><a class="wordwrap wordwrap-it-'+key+'" href="#">'+options.more_text+'</a>');
                        return;
                    });
                }
            }
        });
    }

    $('.wordwrap').wordwrap({'type': 'slide', 'length': 75});

    }); 

    </script>

</div>

<script>
$(document).ready(function(){
jQuery.fn.wordwrap = function(){ 
    
    // Get Args
    var args = arguments[0] || {}; 
    
    // Default Options
    var options = { 
        'type': 'slide', 
        'length': 50, 
        'more_text': 'Show More', 
        'hide_text': 'Hide More', 
        'ellipsis': '...' 
    } 
 
    // Merge defaults with args
    options = $.extend({}, options, args);
 
    $.each($(this), function(key,value){
        var str = $(value).html();
        if(str.length > options.length){
            var display = str.substr(0, options.length-3);
            var displayFill = str;
            $(value).html(display).append(options.ellipsis);
 
            if(options.type == 'slide'){
                $(value).append('<br /><a class="wordwrap wordwrap-it-'+key+'" href="#">'+options.more_text+'</a>');
                $(document).delegate('a.wordwrap-it-'+key, 'click', function(e){
                    e.preventDefault();
                    if($(value).find('a.wordwrap-it-'+key).text() == options.more_text){
                        $(value).html(displayFill).append('<br /><a class="wordwrap wordwrap-it-'+key+'" href="#">'+options.hide_text+'</a>');
                        return;
                    }
 
                     $(value).html(display).append(options.ellipsis).append('<br /><a class="wordwrap wordwrap-it-'+key+'" href="#">'+options.more_text+'</a>');
                    return;
                });
            }
        }
    });
}

$('.wordwrap').wordwrap({'type': 'slide', 'length': 75});

}); 

</script>

Enjoy!