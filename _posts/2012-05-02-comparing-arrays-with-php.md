---
layout: post
permalink: /comparing-arrays-with-php
title: "Comparing Array's with PHP"
category: ["uncategorized"]
tags: array_diff array_diff_assoc array_merge compare-arrays php-2 php5 php5-4
---
# Extensive Array Compare
[php] function array\_compare($weighted, $comparable){ return array\_merge(array\_diff\_assoc($weighted, $comparable), array\_diff\_assoc($comparable, $weighted)); } [/php]