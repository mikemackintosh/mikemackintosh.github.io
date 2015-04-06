---
layout: post
permalink: /understanding-shift-bits-in-php
title: "Understanding Shift Bit's in PHP"
category: PHP
tags: and anding bitwise bitwise-operators left-shift operators or oring php-2 php5 right-shift shift shifting xor xoring
---
### Introduction

A co-worker of mine is attending a programming class and had bitwise operators tossed at them. With their subnetting background, they were able to grasp the AND'ing, OR'ing and XOR'ing. The shift bit's were a bit of a shocker though. 

## When To Divide or to Multiply
The easiest way to remember which direction is multiplication or division is to remember that you need to multiply less than, and divide greater than. In other words, if it's smaller, you want to get big, to do so, multiply. If you're larger, you want to divide.

## What to Shift

The integer to the right of the shift operator is how many bits you need to shift. Not many people can count binary in their head. Those that can't have shared tricks which saves time and energy. 2 ^ n equals the number of bit's that you need to shift. This is the number of bits to shift. Example: 

```
echo 4 << 3; // equivalent to 4 x ( 2 ^ 3) = 32
// 100 shift 3 bits left = 100000 = 4 x 8 = 32
```

## Putting it Together 

```
echo 5 << 5; // equivalent to 5 x ( 2 ^ 5) = 160
// 101 shift 5 bits left = 10100 = 5 x 32 = 160

echo 255 >> 3; // equivalent to 255 / ( 2 ^ 3 ) = 31 
// 11111111 shift 3 right = 11111 = 255 / 8 = 31 
```

Hopefully this helps explain bit shift's and save you some headache in the future.