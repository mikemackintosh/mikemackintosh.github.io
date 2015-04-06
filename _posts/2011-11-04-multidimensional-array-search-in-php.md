---
layout: post
permalink: /multidimensional-array-search
title: "Multidimensional Array Search in PHP"
category: "PHP"
tags: array arrays find-array-value find-in-array multi-arrays multidimensional multidimensional-arrays php-2 php-array-value-search php-arrays php-find-in-array php-find-in-value-array php-multidimensional-array-search php-search-array php-search-array-value php-search-arrays search-arrays
---
# Working with Arrays

Array's are a great way to logical group and order large sets of data. Recently, I was working on a project k

To support the search functionality, I figured there would be no easy way to just use `array_filter` and `array_search`. Resource cost was also a big factor; `for`'s and `foreach`'s just wouldn't work.

Below you will find a `multidimensional_array_search` function which will return all the arrays that contain the search string within it.

# The Function:

The goal of the function is to return the parent array for what the content was found in:

    / ******************************* 
    * array_multi_search 
    * 
    * @array array to be searched 
    * @input search string 
    * 
    * @return array(s) that match 
    ****************************** /  
    function array_multi_search($array, $input){  
        $iterator = new RecursiveIteratorIterator(new RecursiveArrayIterator($array));  
    
    
        foreach($iterator as $id => $sub){ 
            $subArray = $iterator->getSubIterator(); 
                if(@strstr(strtolower($sub), strtolower($input))){ 
                    $subArray = iterator_to_array($subArray); 
                    $outputArray[] = array_merge($subArray, array('Matched' => $id)); 
                } 
        } 
    
    
        return $outputArray; 
    }

The above function is a smorgasburg of snippets I found on the [PHP.Net](http://php.net) docs and [stackoverflow](http://stackoverflow.com). I optimized it to return the array that is matched and the Match string to make it as useful as possible. Give it a go, and let me know how it works.

# Example:

To test this code, you can use the following sample code:

    $array = array( "a" => array( 
                        "b" => array( 
                            "c" => array( 
                                "d" => array( 
                                    "e" => "HighOnPHP" 
                                 ) 
                             ) 
                         ), 
                         "1" => array( 
                             "Two" => 3 
                          ), 
                         "CCD" => array( 
                             "DFfsdf" => array( 
                                 "HighOnPHP" 
                              ), 
                         ), 
                    ),
                    "A" => array( 
                        "Twelve" => 3 
                    ), 
                    "Another" => "HighOnPHP" 
                 ); 
    
    
    print_r(
        array_multi_search($array, 'HighOnPHP')
    ); // [Another], [CCD] and [A] 
    
    
    print_r(
        array_multi_search($array, 'e')
    ); //Returns [A], [Another] and [a][b][c][d][e]

# Summary

This is not the best solution for this, but it works. It's simple user-land code and gets the job done. If you have a better solution please share!

