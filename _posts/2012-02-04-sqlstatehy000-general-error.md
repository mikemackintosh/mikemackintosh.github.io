---
layout: post
permalink: /sqlstatehy000-general-error
title: "SQLSTATE[HY000]: General error"
category: "MySQL"
tags: general-error hy000 mysql-2 mysql-pdo mysqli pdo perconda-pdo php-2 php-pdo sqlstate sqlstatehy000-general-error
---
### Introduction
Wanted to add a quick note on the following error. You will get this when trying to do a fetch on a non-fetchable query. Example, if you try to do an INSERT, UPDATE or DELETE, and run a fetch() or fetchAll() on the result, it will throw a General Error.

## The Error

```
PDOException Object ( 
  [message:protected] => SQLSTATE[HY000]: General error 
  [string:Exception:private] => 
  [code:protected] => HY000 
  [file:protected] => /usr/local/glaze/Zepnik/Core/Database.php 
  [line:protected] => 242 [trace:Exception:private] => Array (
  ... 
)
```

## A Fix

Let it be known that this is one of many fixes: 

```
<?php 
try{
    $res = $pdo->fetch(); 
} 
catch(PDOException $e){ 
  // Silence is golden: $e->getMessage(); 
}
```