---
layout: post
permalink: /jquery-visual-timer-using-knob
title: "Visual Timer using knob"
category: jQuery
tags: countdown javascript jquery-knob jquery-visual-countdown knob timer visual-countdown
---

### Introduction

I do a lot of work on dashboard and reporting. I feel there is a very important emphasis on the quality of the visuals used for reporting and the better they are, the more receptive the data.

A project was recently tossed on my plate where I had to display percentages calculated by a database. Based on previous experience, a Gauge Graph would work perfectly. I wanted one that was compatible with jQuery, wasn't flash based, utilized not-so-eye-pleasing graphics, and something that was extendable and customizable. Not too far into the search, I stumbled upon [jQuery Knob](https://github.com/aterrien/jQuery-Knob). The link will take you to their GitHub page.

jQuery Knob is very well documented and performs astoundingly. One of the things I was able to slap together for a refresh indicator was a Knob Timer. You would provide some settings much like the stock knob, and it would count down and perform an action.

## Function:

I thought this could be helpful for others, so here is the function:

    $.fn.timer = function( userdefinedoptions ){ 
        var $this = $(this), opt, count = 0; 
    
    
        opt = $.extend( { 
            // Config 
            'timer' : 300, // 300 second default
            'width' : 24 ,
            'height' : 24 ,
            'fgColor' : "#ED7A53" ,
            'bgColor' : "#232323" 
            }, userdefinedoptions 
        ); 
    
    
        $this.knob({ 
            'min':0, 
            'max': opt.timer, 
            'readOnly': true, 
            'width': opt.width, 
            'height': opt.height, 
            'fgColor': opt.fgColor, 
            'bgColor': opt.bgColor,                 
            'displayInput' : false, 
            'dynamicDraw': false, 
            'ticks': 0, 
            'thickness': 0.6 
        }); 
    
    
        setInterval(function(){ 
            newVal = ++count; 
            $this.val(newVal).trigger('change'); 
        }, 1000); 
    };

## Usage:

To apply the timer to an element, just issue is like below:

    $('.example').timer();

