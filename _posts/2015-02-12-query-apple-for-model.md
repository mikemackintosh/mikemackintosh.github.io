---
layout: post
title: "Get An Apple Model Name from the Serial Number"
category: "How To"
tags: convert serial apple macbook imac air nokogiri xml httparty
redirect_from:
 - /system-administration/2015/02/12/query-apple-for-model/
---

### Introduction

In TitanOSX, I use the serial number of the device as a unique key/index. This is because there should always be a unique serial number. Part of the reason is, just like a MAC address or Zip Code, each segment of the number represents something specific.

For example, with Apple products, the last 4 of the serial number can be used to get the model type, such as `Mac Book Pro Mid-2014` or `iMac Mid-2012`, etc. 


## Sending the Request

It is very simple to achieve this, assuming `serial` is the Apple product serial number:

{% gist mikemackintosh/0f175a3e2c240a62cf8d example.rb %}

For your convenience, I have included the expected response from Apple:

{% gist mikemackintosh/0f175a3e2c240a62cf8d response.xml %}


## Conclusion

Since the response is simply XML, you can easily adapt this to any language you need. Just remember that you need the LAST 4 of the serial only, otherwise you'll get thrown an error.