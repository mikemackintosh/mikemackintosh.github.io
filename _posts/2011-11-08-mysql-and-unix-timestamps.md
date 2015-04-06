---
layout: post
permalink: /mysql-and-unix-timestamps
title: "MySQL and Unix Timestamps"
category: "MySQL"
tags: date databases linux-system-administration mysql-databases date-format epoc epoch mysql-time mysql-timestamp seconds time timestamp unix unixtimestamp
redirect_from:
 - /mysql-and-unix-timestamps/trackback/
 - /mysql-and-unix-timestamps/feed/
 - /mysql-and-unix-timestamps/
---
# MySQL and Unix Timestamps

There has been a lot of infighting lately about the best way to handle timestamps with MySQL. The most positive and widely accepted answer is simple, whatever works best for the applications.

Personally, I will always choose Unix timestamps versus anything else. I do believe it is the simplest way to interpret time, an ever increasing value across a single axis. String interpretation loses some of the visibility into visualization due to all the differences in locale's and can be using more processor cycles to convert for a simple equation. I believe it's quicker and easier to convert to a string for human interpretation if needed rather than vice-verse.

# Handling Timestamps

For storage type on Unix Timestamps, I use a simple `int(11)`. Some people will complain it's not optimal, which they're right. We allocate extra bits in the field than what's actually needed. I mean heck, timestamps aren't even floating in 64-bit territory yet. But, it's simple, quick, effective, and gives your application enough space to grow.

Here are some of the simple ways to convert to/from timestamps:

    // Inserts the unix timestamp into the table 
    INSERT INTO example (Timestamp) VALUES (unix_timestamp());

You can even save yourself from doing `date()` and parse in MySQL directly using `from_unixtime()`:

    // Return timestamp as a string 
    // Usage: SELECT FROM_UNIXTIME(<unixtimestamp>, <format>) FROM dual; 
    SELECT FROM_UNIXTIME(unix_timstamp(), '%b %D, %Y'); 
    // Nov 8th, 2011

# Formatting Options

The values for the format are as follows:

    Specifier Description %a Abbreviated weekday name (Sun..Sat) %b Abbreviated month name (Jan..Dec) 
    
    
    %c Month, numeric (0..12) 
    %D Day of the month with English suffix (0th, 1st, 2nd, 3rd, …) 
    %d Day of the month, numeric (00..31) 
    %e Day of the month, numeric (0..31) 
    %f Microseconds (000000..999999) 
    %H Hour (00..23) %h Hour (01..12) 
    %I Hour (01..12) 
    %i Minutes, numeric (00..59) 
    %j Day of year (001..366) 
    %k Hour (0..23) %l Hour (1..12) 
    %M Month name (January..December) 
    %m Month, numeric (00..12) 
    %p AM or PM 
    %r Time, 12-hour (hh:mm:ss followed by AM or PM) 
    %S Seconds (00..59) %s Seconds (00..59) 
    %T Time, 24-hour (hh:mm:ss) 
    %U Week (00..53), where Sunday is the first day of the week 
    %u Week (00..53), where Monday is the first day of the week 
    %V Week (01..53), where Sunday is the first day of the week; used with %X 
    %v Week (01..53), where Monday is the first day of the week; used with %x 
    %W Weekday name (Sunday..Saturday) 
    %w Day of the week (0=Sunday..6=Saturday) 
    %X Year for the week where Sunday is the first day of the week, numeric, four digits; used with %V 
    %x Year for the week, where Monday is the first day of the week, numeric, four digits; used with %v 
    %Y Year, numeric, four digits 
    %y Year, numeric (two digits) 
    %% A literal “%” character 
    %x x, for any “x” not listed above

# Example

Let's say you have a database with Unix Timestamps in the column `date`. You can sort and group by Month, Day, and Year by doing something like:

    SELECT COUNT(*) as Posts, 
           FROM_UNIXTIME(date, "%m") as Month, 
           FROM_UNIXTIME(date, "%d") as Day, 
           FROM_UNIXTIME(date, "%Y") 
    FROM Posts 
    ORDER BY FROM_UNIXTIME(date, "%m") as Month, 
             FROM_UNIXTIME(date, "%d") as Day, 
             FROM_UNIXTIME(date, "%Y")
    GROUP BY FROM_UNIXTIME(date, "%m") as Month, 
             FROM_UNIXTIME(date, "%d") as Day, 
             FROM_UNIXTIME(date, "%Y");

The output would look something like:

    Posts | Month | Day | Year
    --------------------------
     12 | 1 | 1 | 2012
     210 | 1 | 18 | 2012
     97 | 2 | 3 | 2012
     1 | 3 | 7 | 2012
     93 | 5 | 6 | 2012
     100 | 5 | 7 | 2012

