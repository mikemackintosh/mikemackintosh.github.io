---
layout: post
permalink: /php-fatal-error-call-time-pass-by-reference-has-been-removed-in
title: "PHP Fatal error:  Call-time pass-by-reference has been removed in"
category: "Gotcha"
tags: var call-time class fatal-error functions pass-by-reference php-2 php-fatal-error reference removed
---
### Working with References

Starting in PHP 5.3.0, a lot of people began having issues with code written for previous versions. The reason for this is because a feature known as `call-time pass-by-reference` was deprecated. For the sake of consistency, the ability to add the reference symbol (`&`) to a function call (`myFunc( &$a )`) was deprecated.

> There is no reference sign on a function call - only on function definitions. Function definitions alone are enough to correctly pass the argument by reference. As of PHP 5.3.0, you will get a warning saying that `call-time pass-by-reference` is deprecated when you use & in foo(&$a);.
> 
> See: [PHP: References](http://php.net/manual/language.references.pass.php)

When you work with a large code-base, and you required the reference to be passed at the function call level, your resulting variable will be skewed if you don't reference the variable on call. By moving the reference notation to the function definition, you remove the possibility of user error when calling your function, ensuring the reference is passed every time the function is called.

## Examples

Notice the difference as to where the reference (`&`) is placed.

#### Good

    // PHP 5.3 and newer
    function myFunc( &$arg ) { 
        // do something
    } 
    
    
    myFunc( $var );

#### Bad
    
    // Pre-PHP 5.3
    function myFunc($arg) { 
        // do something
    } 
    
    
    myFunc( &$var );

