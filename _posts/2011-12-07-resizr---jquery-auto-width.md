---
layout: post
permalink: /jquery-resizr
title: "ResizR - jQuery Auto-Width"
category: "jQuery"
tags: auto-change-width-jquery div dynamic-width-java dynamic-width-javascript html5 jquery-2 jquery-change-width jquery-custom-fucntion jquery-div-width jquery-dynamic-width jquery-function jquery-page-width jquery-resolution-width jquery-set-div-width width
---
# Background: The Need For Resizr
I have been working on some projects latley which consist of an aside and a section. Those who have been missing out on HTML5, think of it as a workspace with a toolbar. The left pane has a static width while the right pane needed to be 100% of the remaining space. Essentially, there was no easy way to accomplish this unless I decided to use a table. In DIV world, it's not so easy. Below I posted the function incase you have a need for Resizr too!

# The Function: Resizr

		<script>
		$(document).ready(function(e){
			/ ***Start Resizr*** /
				$.fn.resizr = function(){
				var offset = arguments[0] || {}; // It's your object of arguments

				$('.resizr').width($(window).width()-offset);
				$('section').height($(window).height());
							
							
				$(window).resize(function(e) {
					$('.resizr').width($(window).width()-offset).height($(window).height());
					$('section').height($(window).height());
				});
			};				
			/ ***End Resizr*** /
		});
		</script>

Usage is simple, call the following within document.ready and assign your right pane to class resizr:

     $.resizr(256); // Where 256 is the offset of the left pane

