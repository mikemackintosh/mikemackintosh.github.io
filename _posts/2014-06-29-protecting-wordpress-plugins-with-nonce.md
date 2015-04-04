---
layout: post
permalink: /protecting-wordpress-plugins-nonce
title: "Protecting Wordpress Plugins with nonce"
category: "How To"
tags: csrf nonce security wordpress php
---

## Introduction

A [Cross-Site Request Forgery](http://en.wikipedia.org/wiki/Cross-site_request_forgery) is a type of malicious activity that can occur while surfing the web. It uses a user trusted website/domain to trick the user into clicking a link or supplying data which could be used to formulate a URL that executes some kind of action. If you take a look at the [OWASP](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ( [Open Web Application Security Project](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) page for CSRF's they give some good examples as to some of the ways they exist.

In Wordpress land, CSRF's can do things like allow an attacker to hijack certain plugin actions, execute commands like delete all posts, add a new user, and some other possibly dangerous things.

### Protecting Yourself

A CRSF token creates a one-time-use token, that would be very close to impossible to guess, and is sent with the request. Once the request is sent to the server, it is validated, and confirmed. If it fails, you can display a message or log application details for research.

For the longest time, Wordpress was known as a hackers playground and had a reputation of laughter in the security community. Luckily, they are turning around and supplying some helper functions like, `wp_*_nonce`, which is a family of commands that create and validate `nonce` keys, or CSRF Tokens.

### Using nonce in Wordpress

As we explained earlier, there are two steps to using a CSRF token, the creation and verification. Once you create the token, you would append it as, most commonly, a hidden attribute in your `<form></form>`. This key would be sent with your `POST` request.

    <input name="dtr_user_attrib" type="hidden" value="<?php echo wp_create_nonce('dtr_user_attrib'); ?>" />

Inside of your `POST` action target, check to make sure the `nonce` was supplied:

    if (!isset($_POST['dtr_user_attrib'])) {
        die("This form was not submitted with appropriate security content.");
    }
    else if (!wp_verify_nonce($_POST['dtr_user_attrib'],'dtr_user_attrib')) {
        die("This form was not submitted with appropriate security content.");
    }
    
    
    // Execute your code here

Enjoy.

