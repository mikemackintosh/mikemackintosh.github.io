---
layout: post
permalink: /javascript-ucfirst-function
title: "Implementing ucfirst in JavaScript"
category: "One-Liner"
tags: capitalize javascript jquery ucfirst
---
### What is ucfirst()?

`ucfirst` is a function which exists natively in [PHP](http://php.net/manual/en/function.ucfirst.php) and other languages. It is used to capitalize the first character of a string. Unfortunately, it is not included in the JavaScript language by default.

That's alright, because it is not that difficult to emulate!

## Function: ucfirst

The `ucfirst` function will retrieve the character at position 0 in a string an apply the uppercase function to it. It will then apply a lowercase function and concatenate the remainder of the string to the uppercase first character.

With JavaScript, it is very simple as you can see below:

    function ucfirst(string){ 
        return string.charAt(0).toUpperCase() + string.slice(1).toLowerCase(); 
    }

## Usage

    // Create example string variable
    var example_str = "this is an example string!";
    
    
    // Lets view the output
    console.log( ucfirst( example_str ) );
    
    
    // Returns:
    // This is an example string!

