---
layout: post
permalink: /allowing-multiple-accounts-email-wordpress
title: "Allow Multiple Accounts with Same Email in WordPress"
category: "How To"
tags: account email multiple-emails registration wordpress
redirect_from:
 - /allowing-multiple-accounts-with-same-email-in-wordpress
---
### Introduction

I was working on a project where a users would register their family to a modified WordPress instance, and fill out additional User Meta. Each family member would have their own user account, but not all the time did each family member have their own email. For this reason, I needed a way to register multiple accounts with the same email. I Googled, but all the results were the same, use a `pre_user_email` hook. The problem with this is, in 3.9, the `pre_user_email` hook was never fired on account registration.

> *Note*: You will want to add this to your `functions.php` file in either your `plugin` or `theme`.

## The Documented Wrong Way

According to the documentation, the following should work out of the box:

    // This hook should run before user email validation
    add_filter( 'pre_user_email', 'skip_email_exist');
    /**
     * [skip_email_exist description]
     * @param [type] $user_email [description]
     * @return [type] [description]
     */
    function skip_email_exist($user_email){
    
    
        define( 'WP_IMPORTING', 'SKIP_EMAIL_EXIST' );
        return $user_email;
    }

But in Wordpress 3.9.1 it wasn't working. This function was never called on account registration, only on account import.

## The Right Way

I had to use the `registration_errors` hook to get it working. Here, I was able to unset the `email_exists` key from the `$errors` object and the account was added as expected.

    // Hook run on registration errors
    add_filter( 'registration_errors', 'dtr_registration_errors', 10, 3);
    /**
     * Removes email_exists error
     * 
     * @since 1.0
     * 
     * @param [type] $errors [description]
     * @param [type] $sanitized_user_login [description]
     * @param [type] $user_email [description]
     * @return [type] [description]
     */
    function dtr_registration_errors ($errors, $sanitized_user_login, $user_email) {
    
    
        if( isset($errors->errors['email_exists'])) {
            unset($errors->errors['email_exists']);
        }
    
    
        return $errors;
    }

Enjoy.

