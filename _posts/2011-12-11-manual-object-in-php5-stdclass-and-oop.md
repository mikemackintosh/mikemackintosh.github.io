---
layout: post
permalink: /manual-object-in-php5
title: "Manual Object in PHP5 (stdClass and OOP)"
category: "PHP"
tags: array non-object notice object oop php-2 php-oop php4 php5 stdclass stdobject trying-to-get-propert trying-to-get-property-of-non-object-in type-casting typecasting unknown warning
---
## Background: NULL Object
When you have a NULL object and try to manually assign a variable to it, such as, `$object->user->name`, but `$object->user` is not a class object, you will receive the following error: 

**[Notice]: Trying to get property of non-object in [file]**. To fix this, follow the few simple steps below:

## The Code: Creating an Object

    <?php 
    $object = new stdClass;
    $object->data = (object)array(); 
    $object->data->item = 'item'; 
    echo $object->data->item;