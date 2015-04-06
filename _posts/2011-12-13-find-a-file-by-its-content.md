---
layout: post
permalink: /how-to-find-file-by-content
title: "Find A File By It's Content"
category: "How To"
tags: body content content-of-file linux file file-file-by-content file-find-in filename find-contents-in-file find-file-name-by-content find-filename find-in-file linux-find-content-from-file linux-find-in-file linux-find-witihin-file return return-filename-from-content return-filename-with-body search search-file-by-content search-filename-with-content search-filesystem-for-content search-for-file-with-content search-for-file-with-in-file search-within-file
---

### Introduction
Within Linux, there are hundreds of ways to find a file. It all depends on what you are most comfortable using. Back in the day, located was the poison of choice, but after learning find, it was soon replaced. Find is a very powerful utility, especially when it comes to looking for specific files.

## The Search
The basic syntax is as follows: 

    find /directory -name "\*" -print |xargs grep 'searchstring' 

Here is an example looking for which file the `/usr/local/apache-2.2` directory contains the word `Directory`: 

    find /usr/local/apache-2.2/ -type f -print |xargs grep "Directory" | head 6 
    Binary file /usr/local/apache-2.2/bin/httpd matches 
    /usr/local/apache-2.2/bin/apxs: error("Directory `$name' already exists. Remove first"); 
    /usr/local/apache-2.2/include/http_config.h: NO_ARGS, /** Directory **/ 
    /usr/local/apache-2.2/include/http_config.h: #define OR_LIMIT 1 /** <Directory> or <location> **/
...

**Note:** If you are looking for a file that requires root privileges (outside your allowed directories), you can use `sudo` before the words find and `xargs`. If you don't, you will get flooded with 'Permission Denied' and 'No such file or directory' error's:

    sudo find /usr/local/apache-2.2/ -type f -print |sudo xargs grep "Directory"


### Update - 4/12/2012
If you have a file with spaces in it, the above will fail. To remedy this situation use the following syntax:

    find . -print0 | xargs -0 grep "what to grep for"

Since a filename with spaces will be seen as multi-arguments, grep will fail with file not found. Suffixing print with 0 sets the output to argument 0 for find, and adding the 0 option to xargs calls specifically argument 0.