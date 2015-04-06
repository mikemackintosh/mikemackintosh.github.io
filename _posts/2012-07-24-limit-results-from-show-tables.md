---
layout: post
permalink: /mysql-limit-results-from-show-tables
title: "Limit Results From SHOW TABLES "
category: "MySQL"
tags: limit mysql-2 show-tables tables
---
### Someone asked, We answered

Not too long ago, a user asked on [StackOverflow: How to limit SHOW TABLES query](http://stackoverflow.com/q/11635769/1431239). Essentially, the user wanted to know how to limit the number of results from `SHOW TABLES`. Being nose deep in MySQL just about everyday, we thought we would take a stab at answering.

Unfortunately, `SHOW TABLES` does support the `LIMIT` constraint. Although this would be the simplest, and most logical way to do it, you can't due to limitations with MySQL. So, we thought we would use the next best thing: `INFORMATION_SCHEMA`.

## Information Schema

A lot of coders always see the `INFORMATION_SCHEMA` database, but not many actually use it. Some of the information you can get out of it includes, column names, triggers, views, statistics, engines and more.

In reality, most people probably won't have a need for it. Keep in mind that there is always an exception, especially in very large projects.

## Limiting Table Searches

To achieve this though, you need to use the following:

    SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES 
    WHERE TABLE_SCHEMA = '<DB_TO_SEARCH>' 
    AND TABLE_NAME LIKE "table_here%" 
    LIMIT 0,5;

You need to set two strings:

    <DB_TO_SEARCH> the database you want to search and 
    <TABLE_PREFIX> which is the you want to match, or omit it

