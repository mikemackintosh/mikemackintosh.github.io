---
layout: post
permalink: /gotcha-wordpress-option-form-not-posting
title: "Gotcha: Wordpress Option Form Not Posting"
category: ["uncategorized"]
tags: action form form-not-posting gotcha options strrpos substr wordpress wordpress-admin-page-not-posting wordpress-form-not-posting
---
# The Issue - Form Not Posting
I was working on creating a theme with an Admin Options page for WordPress but ran into an issue from [the article I was following](http://www.1stwebdesigner.com/wordpress/how-to-create-an-options-page-for-your-wordpress-theme/) where the form wouldn't post. Normally, this is because there is either no action, or no interpreter for the \_GET or \_POST. Knowing this, I noticed there was no form action. Easy fix. With WordPress, your actions are managed by your URI, example: [plain]function.php?page=called.php[/plain] When you bring that to different environments, not all servers will handle the request within the same $\_SERVER variable. S o what's the best way to POST your form the page serving you?
# The Fix
You can fix it by using a combination of substr and strrpos. substr, or sub string, will allow you to return only the characters within the specified offsets. strrpos, or string reverse position, will return the first integer position of the character you specified, starting from the end of the string. Understanding your URI, ex: http://www.highonphp.com/wordpress/wp-admin/admin.php?page=functions.php, you know that the page you need to post your form to is _admin.php?page=functions.php_. So, by using the following function, we will get the position of the / after _wp-admin/_ and use that as the starting point for substr. [php] echo substr($\_SERVER['REQUEST\_URI'], strrpos($\_SERVER['REQUEST\_URI'], '/')+1); [/php] When you put it all together the FORM tag looks like this: [php]<form method="post" action="&lt;?=substr(%24_SERVER%5B'REQUEST_URI'%5D,%20strrpos(%24_SERVER%5B'REQUEST_URI'%5D,%20'/')+1);?&gt;">
[/php]

Enjoy.</form>