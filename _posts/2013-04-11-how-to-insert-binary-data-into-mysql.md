---
layout: post
permalink: /how-to-insert-binary-data-into-mysql
title: "Insert Binary Data into MySQL"
category: "How To"
tags: bin2hex binary db hex hex2bin inet_ntop inet_pton ip mysql php
---
### Binary Data is AWESOME!

Working with binary data can be extremely beneficial, especially when it's very detailed data. Example, it is easier to `&` and `|` two integers than to create a crazy function to do math. In one of our previous posts, we created a class and a few functions to handle IP addresses. Inserting a converted IP into the database allows you to `&` and `|` a user supplied IP to return matches. This is a great way to work with IP's within databases.

One issue you may run into is, when you echo a binary string, it could result in `a ©'`. This doesn't look like anything readable in this format, but if you convert this with `inet_ntop`, it would return `97.32.169.39`.

If you tried to insert that specific string into a database, it would fail under the most simplest of `INSERT`'s. Note the `'` (single quote). We will provide some examples on how to insert this string using queries and PHP.

## Our Sample Database

For this example, we will use the following database and table:

    CREATE DATABASE BinaryTest;
    
    
    CREATE TABLE `BinaryTest`.`Testing` (
      `binary_field` varbinary(39) NOT NULL
    );

## Inserting the Data

We have included some examples on how you could insert binary data. There are 2 simple ways to do this, and one that is just plain cool.

**1st Method** : Prepared Statements

Our first method will use a prepared statement. This will sanitize your input and gracefully insert the row.

    try{
        echo "Trying Prepared...\n";
        $stmt = $pdo->prepare("INSERT INTO Test.Testing SET binary_field=?");
        $stmt->execute(array(inet_pton("97.32.169.39")));
    }
    catch(\Exception $e){
        echo $e->getMessage()."\n";
    }

**2nd Method** : Escaping Statements

Those who are old-timers with MySQL know about the need to quote your insert strings. This would turn `'` and `"` into `\'` and `\"`, preventing the breakage of quoted values. This will work, but not the most secure and desirable method.

    try{
        echo "Trying Prepared...\n";        
        mysql_query("INSERT INTO Test.Testing SET binary_field=". mysql_real_escape_strings(inet_pton("97.32.169.39"));
    }
    catch(\Exception $e){
        echo $e->getMessage()."\n";
    }

**3rd Method** : Converting to Hex

This is the cool method. If you want to insert Binary data into a MySQL table, convert it to hex. You can then pass this to your query and it will convert it to binary upon insertion.

    try{
        echo "Trying Hex...\n";
        $pdo->query("INSERT INTO Test.Testing SET binary_field=0x". bin2hex( inet_pton("97.32.169.39") ) );
    }
    catch(\Exception $e){
        echo $e->getMessage()."\n";
    }

## Summary

We would always recommend someone to prepare their query first. If you can't, go with the 3rd option. You can probably clean it up and optimize the code some more, but try to stay away from manually escaping your values.

