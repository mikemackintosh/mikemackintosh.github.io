---
layout: post
permalink: /obfuscating-email-addresses-perl
title: "Obfuscating Email Addresses in Perl"
category: "Privacy"
tags: privacy perl 
---
### Introduction

I was working on a project recently, and for a username, the email address was displayed. This was the only personal information stored in a database as well. Their username would only be visible on the top-right of the page if they were logged in.

## So what's the problem?

If their session gets hijacked, the attacker then has access to their email address. Many sites and frameworks that power those sites use very lazy methods for tracking sessions. They don't validate IP addresses, User-Agents, or other unique identifiers to help enforce sessions. If someone was to brute force your session ID, or even steal your cookies and replay them, your password would be pointless.

## The Code

Here is an example where `$user->email` is pulled from a database.

    my $user_email = $user->email;
    my $replace_substring = substr($user_email, floor(index($user_email, "\@")/2), index($user_email, "\@"));
    $user_email =~ s/$replace_substring/.../s;
    
    
    // do what you want with $user_email

This will turn `email@domain.com` into `em...@domain.com`.

