---
layout: post
permalink: /?p=735
title: "Bitwise Operators"
category: ["uncategorized"]
tags: 
---
# Purpose of Bitwise Operators

Bitwise Operators allow the manipulation of integers at the bit-level. The operators allow you to logically evaluate your integers to determine if there is only one bit or a specific bit set, bit shifting to increase numbers integers by the power of two and more.

Here is the definition per Wikipedia:

> A bitwise operation operates on one or more bit patterns or binary numerals at the level of their individual bits. It is a fast, primitive action directly supported by the processor, and is used to manipulate values for comparisons and calculations. On simple low-cost processors, typically, bitwise operations are substantially faster than division, several times faster than multiplication, and sometimes significantly faster than addition.
> 
> While modern high-performance processors usually perform addition and multiplication as fast as bitwise operations, [1](http://en.wikipedia.org/wiki/Bitwise_operation) the latter may still be optimal for overall power/performance due to lower resource use. - [Wikipedia](http://en.wikipedia.org/wiki/Bitwise_operation)
# Real Life Use

The foundation of networking revolves around the application of bits. With IPv4 there are 4 octets with 8 bits; with IPv6 there are 8 segments with 16 bits in each. Many network administrators subnet, supernet, and even VLSM (Variable Length Subnet Mask) by hand. It requires the use of AND'ing, OR'ing, NEG'ing, XOR'ing and Bit Shifting.

