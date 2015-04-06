---
layout: post
permalink: /jquery-creating-a-hover-timeout
title: "Creating A Hover Timeout"
category: "jQuery"
tags: hover hover-timeout javascript-mouseover jquery-2 mouseout mouseover timeout
---
### Introduction
I came across the need to detect and delay a hover before executing a function. The page was full of images, and when hovered over, would create an AJAX request to the server and display in a popup. 

## A Quick Example
Here is an example on how to achieve it. 

```
$('img.expand').bind({ mouseover: function() { 
	obj = $(this); 
	jQueryTimeout = setTimeout(function() { 
		$.post('/target-uri', 
		{ 'attribute1':obj.attr('title'), 'attribute2':obj.attr('href') }, 
		function(response){ 
			console.log(response); 
		}, 
		'json' ); 
		}, 
		2000); 
	}, 
	mouseleave: function() { 
		clearTimeout(jQueryTimeout); 
	} 
});
```

## Demo
In the demo, there is a link, 'here', and if you hover over it for 2 seconds, it will display the title attribute in the input text field below.

<script type="text/JavaScript">
$(document).ready(function(){
    $('.demo').bind({
		mouseover: function() {
			obj = $(this);
			hoverTimeout = setTimeout(function() {
				$('.demo-target').val(obj.attr('title'));
			}, 1500);
		},
		mouseleave: function() {
			clearTimeout(hoverTimeout);
		}
	});
});
</script>

This is a demo. Hover <a href="#" title="This SHould Now Appear In Textarea" class="demo">here</a>:

<input type="text" class="demo-target" value="">

## Demo Code
```
<script>
$(document).ready(function(){
    	$(document).ready(function(e) {

		mouseover: function() {
			obj = $(this);

			jQueryTimeout = setTimeout(function() {
				$('.demo-target').val(obj.attr('title'));
			}, 2000);
		},
		mouseleave: function() {
			clearTimeout(jQueryTimeout);
		}
	});
});
```