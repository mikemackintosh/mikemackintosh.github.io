---
layout: post
permalink: /cli-coloring-with-php
title: "PHP CLI Color"
category: "PHP"
tags: bash color-app-logging color-bash-php color-bash-prompt color-cli color-php-cli color-promtp colored-application-logging colored-bash-script-php php-bash-color php-cli php-cli-color php-cli-colors php-color-bash php-color-output-text php-file-colors php-logging shell text-output-color-file xterm
redirect_from:
 - /php-cli-color
---
### Introduction

This is something I have been running in all my scripts for many a years now all the way back to my first Linux install. A friend of mine recently peeked over my shoulder and demanded that I hand over this _Pandora's Box_. One of the most important features of an application is the ability to log and report. How can you be sure your application is working correctly or as designed if you can't see what it is doing? I had an Accounting engine I wrote which would tell me real time who logged in, failed and what pages are being accessed. 

I logged all output from this engine to `/var/log/Accounting`. Within this file were hundreds of thousands of lines with all sorts of user activity which most of I didn't care about. If I needed something specific I would _tag (tail and grep)_ the file. After some time, this lost its effectiveness when in monochrome on a terminal. I needed to spice this up by adding some creative life to the output.


# Coloring Output with PHP

To add color to my PHP script output, I simply added some **[b][/b]** style tags to my `fwrite` or `file_put_contents` and call another function. 

My log output looks something like this: 

```
11/30/2011 16:51:20 - (II) :: sixeightzero [172.31.28.1] Viewing Page [/appname/interface/controller/method]
```

The PHP code I use to generate the above is as follows: 

```
// Define Some Application Provided Vars
$log_type="(EE)"; 
$User = "sixeightzero"; 
$IP = $_SERVER['REMOTE_ADDRESS']; 
$Message = "Log Message Here!"; 

$entry = color_output(date("n/j/Y H:i:s")." - ". $log_type .":: [b]". $User ."[/b]". $IP . ucwords($Message)."\n"); 
fwrite(fopen("file.log", "a+"), $entry); 

function color_output($message){ 
  // Support for white 
  $message = str_replace(array("[b]", "[/b]"), array("\033[1;37m", "\033[0m"), $message); 

  // Support for red 
  $message = str_replace("[b color=red]", "\033[0;31m", $message); 

  // Support for grey 
  $message = str_replace("[b color=grey]", "\031[0;37m", $message); 

  // Support for green 
  $message = str_replace("[b color=green]", "\031[0;32m", $message); 

  // Support for blue 
  $message = str_replace("[b color=blue]", "\031[0;34m", $message); 

  return $message; 
}
``` 

## Conclusion

By using the above, you can simply add the `[b][/b]` tags around what you want to be colored and wrap with the color_output() function.