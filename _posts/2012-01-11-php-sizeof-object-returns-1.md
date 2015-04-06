---
layout: post
permalink: /php-sizeof-object-returns-1
title: "PHP Sizeof Object Returns 1"
category: "Gotcha"
tags: count countable countable-interface lenght-of-object length-of-array obj-size object-length object-size php-array-lenght php-array-size php-count php-countable php-oop-sizeof php-sizeof php5-count php5-countable php5-sizeof size-of-obj size-of-object sizeof
---

### Introduction
One of the more common mistakes I have seen in PHP OOP is the incorrect use of `sizeof`. Unless your class or object implements the [Countable Interface](http://php.net/manual/en/class.countable.php "Countable Interface"), you must convert your object to an array to use `sizeof`/`count` on it. The following is directly from PHP.net's Manual:

> Returns the number of elements in var. If var is not an array or an object with implemented Countable interface, 1 will be returned. There is one exception, if var is `NULL`, `0` will be returned.

### Sizeof/Count Example
Below, we will take a query result, convert to object and typecast to an array:

```
// Query the database and return 20 rows 
$result = $mysqli->query("SELECT Name FROM City LIMIT 20"); 

// Assign the DB results to $obj 
$obj = $result->fetch_object(); 

// Typecast array to $obj 
if( sizeof( (array) $obj ) > 10 ){ 
  echo "$obj is greater than 10"; 
}
```