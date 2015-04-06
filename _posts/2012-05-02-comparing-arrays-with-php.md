---
layout: post
permalink: /comparing-arrays-with-php
title: "Comparing Array's with PHP"
category: PHP
tags: array_diff array_diff_assoc array_merge compare-arrays php-2 php5 php5-4
---
### Introduction
Sometimes you have a need to compare arrays in PHP, that is, getting everything in not in common between two arrays. `array_diff_*` kind of does the job, but if you take a look below, you can find out why it's a more robust solution.

## Get 'er Done

Pass in two args, `$weighted` which takes precedence, and `$comparable` which is what you are comparing against.

```php
function array_compare($weighted, $comparable){ 
  return array_merge(
    array_diff_assoc($weighted, $comparable), 
    array_diff_assoc($comparable, $weighted)
  );
}
```

## Example

Create our test arrays:

```php
$weighted = ['a', 'b', 'c', 'x', 'y', 'z'];
$comparable = ['a', 'c', 'e', 'g', 'y'];
```

Compare them using `array_compare`:

```php
var_dump(array_compare($weighted, $comparable));
array(7) {
  [0]=>
  string(1) "b"
  [1]=>
  string(1) "c"
  [2]=>
  string(1) "x"
  [3]=>
  string(1) "z"
  [4]=>
  string(1) "c"
  [5]=>
  string(1) "e"
  [6]=>
  string(1) "g"
}
```