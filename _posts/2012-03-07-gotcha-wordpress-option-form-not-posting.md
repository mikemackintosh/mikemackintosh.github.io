---
layout: post
permalink: /gotcha-wordpress-option-form-not-posting
title: "Wordpress Option Form Not Posting"
category: "Gotcha"
tags: action form form-not-posting gotcha options strrpos substr wordpress wordpress-admin-page-not-posting wordpress-form-not-posting
---
### Introduction
I was working on creating a theme with an Admin Options page for WordPress but ran into an issue from [the article I was following](http://www.1stwebdesigner.com/wordpress/how-to-create-an-options-page-for-your-wordpress-theme/) where the form wouldn't post. Normally, this is because there is either no action, or no interpreter for the `$_GET` or `$_POST`. Knowing this, I noticed there was no form action. Easy fix. With WordPress, your actions are managed by your URI, example: 

```php
function.php?page=called.php
```

When you bring that to different environments, not all servers will handle the request within the same `$_SERVER` variable. So what's the best way to POST your form the page serving you?

## Putting a Fix In Place
You can fix it by using a combination of `substr` and `strrpos`. `substr`, or sub string, will allow you to return only the characters within the specified offsets. `strrpos`, or string reverse position, will return the first integer position of the character you specified, starting from the end of the string. Understanding your URI, ex: `http://www.highonphp.com/wordpress/wp-admin/admin.php?page=functions.php`, you know that the page you need to post your form to is `admin.php?page=functions.php`. So, by using the following function, we will get the position of the / after `wp-admin/` and use that as the starting point for `substr`. 

```php
echo substr($_SERVER['REQUEST_URI'], strrpos($_SERVER['REQUEST_URI'], '/')+1);
```

When you put it all together the FORM tag looks like this: 

```php
<form method="post" action="<?=substr($_SERVER['REQUEST_URI'], strrpos($_SERVER['REQUEST_URI'], '/')+1);?>">
```
Enjoy.