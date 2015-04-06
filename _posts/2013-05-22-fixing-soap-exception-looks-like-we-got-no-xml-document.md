---
layout: post
permalink: /fixing-soap-exception-no-xml
title: "Fix SOAP Exception: \"looks like we got no XML document\""
category: "How To"
tags: api client integration server soap soap-server soapclient soapserver xml
---
### Working with SOAP

Personally, I hate SOAP. It is a bloated, slow and painful API service, but allows for cross platform support out of the box. It is XML driven and allows you to transports large amounts of data to and from a server.

A SOAP server holds what is called a WSDL file. This is also know as a Web Service Definition Language file. Within this file, XML namespaces, methods, types, parameters and more are all defined to allow a SOAP client to learn how to interpret requests and responses.

A SOAP client will download the WSDL file, and using this file, perform the desired call-to-actions. The definitions within this file will allow the response to be translated into an object, with type assignments and Object assignment. This is great if you are trying to interface with a platform with a lot of logical data that you would like to export.

PHP has a built in extension called [PHP: SoapClient](http://www.php.net/manual/en/class.soapclient.php). To create a connection to a SOAP server, you could call: `$client = new SoapClient($url, array("trace" => 1, "exception" => 0));`, where `$url` is your WSDL file.

## Error Handling

One of the error's you will experience when working with XML is "Looks like we got no XML document". If you sniff on the interface or print the response using `print_r($client->__getLastResponse());` you will most likely to see what appears to be well-formed XML data.

The problem many times ends-up being what they call `BOM`.

> The Byte-Order-Mark (or BOM), is a special marker added at the very beginning of an Unicode file encoded in UTF-8, UTF-16 or UTF-32. It is used to indicate whether the file uses the big-endian or little-endian byte order. The BOM is mandatory for UTF-16 and UTF-32, but it is optional for UTF-8.

This essentially is an extra character usually invisible to the eye that disrupts XML parsers; one of them being the SOAP XML parser.

Unfortunately, there is no easy way to store the SOAP Servers response in a variable after the API call and remove this character. This BOM will cause an exception before you even get that far. There is one simple step you can take to correct the issue.

## The Fix

Instead of accessing the `SoapClient` class directly, extend your own. We are going to override the `__doRequest()` method and apply our BOM removal here.

Example class:

    class SoapClientNG extends \SoapClient{
    
    
        public function __doRequest($req, $location, $action, $version = SOAP_1_1){
    
    
            $xml = explode("\r\n", parent::__doRequest($req, $location, $action, $version););
    
    
            $response = preg_replace( '/^(\x00\x00\xFE\xFF|\xFF\xFE\x00\x00|\xFE\xFF|\xFF\xFE|\xEF\xBB\xBF)/', "", $xml[5] );
    
    
            return $response;
    
    
        }
    
    
    }

This method will receive the response in XML format with HTTP headers, strip the headers, and then remove the BOM characters. The result is returned back to SOAP's inner workings and a response is provided to your SOAP call.

Hopefully this helps a few other people on how to manage and intercept the response.

