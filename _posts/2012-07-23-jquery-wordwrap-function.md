---
layout: post
permalink: /jquery-wordwrap-function
title: "jQuery WordWrap Function"
category: ["coding", "javascript-coding", "jquery-coding", "uncategorized"]
tags: extended function jquery-2 jquery-wordwrap word-wrap wordwrap wordwrap-toggle
---
# WordWrap Extension for jQuery

I was working on a project where I wanted to WordWrap some content and either display an ellipse or add a toggle button to show/hide more. Looking around I could not find anything that did what I wanted, so I tossed together the below. Performance is great and works pretty good.

# The Code

[javascript] jQuery.fn.wordwrap = function(){ var args = arguments[0] || {}; var options = { 'type': 'slide', 'length': 50, 'more\_text': 'Show More', 'hide\_text': 'Hide More', 'ellipsis': '...' } options = $.extend({}, options, args);

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
    
    
            $('.class-to-wordwrap').wordwrap({'type': 'hide', 'length': 75});
            $('.class-to-wordwrap-toggle').wordwrap({'length': 240, 'type': 'slide'});

[/javascript]

Enjoy!

