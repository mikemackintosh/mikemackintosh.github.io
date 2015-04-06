---
layout: post
permalink: /reflections-unsung-heros-of-php
title: "Reflections: Unsung Hero's of PHP"
category: "PHP"
tags: class function method object patterns php-2 php5-3 php5-4 reflection
---
### What are Reflections?

First off, if you don't know what Reflections are, you should take a quick walk through the [PHP: Reflection](http://php.net/manual/en/book.reflection.php "PHP: Reflection") manual. You have been missing out. Reflections are some of the most powerful shadow features in current generation ( >= PHP5.3).

> PHP 5.3 comes with a complete reflection API that adds the ability to reverse-engineer classes, interfaces, functions, methods and extensions. Many PHP developers are familiar with `call_user_func_array()`, but not Reflections. This function allows you to pass arguments to an object of the Callable type, most commonly a function or class method. It was introduced in 4.0.4 and gave developers a new approach to dynamically constructing code. Here is an example to freshen your memories:

    // Example function function 
    example($arg1, $arg2){ 
        echo $arg1; // Prints: argument_a 
        echo $arg2; // Prints: argument_b 
    } 
    
    
    // array with contents to be passed to function 
    $arguments = array("argument_a", "argument_b"); 
    
    
    // pass argument array to function example 
    call_user_func_array('example', arguments);

This may not seem like a big deal, but in some scenarios, you can't always estimate where you get your arguments from _(although some will argue this is bad coding practice)_. If you have a string, like `"this|is|a|string"`, you can `explode` it and pass the parts to `call_user_func_array()`. The string can then be any number of parts long, and if you want, you could catch them in the `example()` function using `func_get_args()`.

## What can Reflections do?

Reflections are very powerful, in many ways. They give a developer the ability to read document comments, dynamically call methods, invoke arguments and more. It takes the ability that `call_user_func_array()`, and allows you to extend it even further.

Here is a simple example using a sample class:

    // Example Class
    class ExampleClassToReflect { 
        private $classData; 
    
    
        public function __construct($arg1, $arg2){ 
            // Construct Called 
            $this->classData = "$arg1 reflected $arg2\n"; 
        } 
    
    
        public function callMethod(){ return $this->classData; } 
    } 
    
    
    $rc = new ReflectionClass('ExampleClassToReflect'); 
    $class = $rc->newInstanceArgs(array('foo', 'bar')); 
    echo $class->callMethod(); // returns 'foo reflected bar'

The above will create an instance of `ExampleClassToReflect` and pass the arguments `foo` and `bar` to it's constructor. You can then use the variable `$class` just as if you called the class using the `new` keyword.

## Conclusion

Staying up on new functions and classes is very important. There are a lot of snippets of [>= PHP5.3] code I have seen where someone uses an `eval()` but had they known about Reflections, they could have saved themselves both headache and vulnerability exposure.

