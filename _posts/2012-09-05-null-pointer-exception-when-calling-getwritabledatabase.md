---
layout: post
permalink: /null-pointer-exception-when-calling-getwritabledatabase
title: "Null Pointer Exception When Calling getWritableDatabase()"
category: Android
tags: android database dbhelper sqlite sqliteopenhelper
---

### Introduction
Regardless if you are a PHP programmer, a C programmer, or a ColdFusion programmer, everyone knows that a NullPointerException is pretty much the most useless error in the book. What is helpful however, is the stacktrace which always follows. What is even more painful is if you are working with the `getWritableDatabase` method for SQLite, and you get the error message. All in all, it means you are referencing or passing the wrong context to the database, but it is not apparent. 

    09-05 14:43:52.537: E/AndroidRuntime(870): java.lang.RuntimeException: Unable to instantiate activity ComponentInfo{com.example/com.example.Activity}: java.lang.NullPointerException ... 09-05 14:40:02.497: E/AndroidRuntime(823): Caused by: java.lang.NullPointerException 09-05 14:40:02.497: E/AndroidRuntime(823): at android.database.sqlite.SQLiteOpenHelper.getWritableDatabase(SQLiteOpenHelper.java:157) 09-05 14:40:02.497: E/AndroidRuntime(823): at com.example.sql.Database.<init>(Database.java:28)
    09-05 14:40:02.497: E/AndroidRuntime(823): at com.example.Activity.<init>(Activity.java:14)
    09-05 14:40:02.497: E/AndroidRuntime(823): at java.lang.Class.newInstanceImpl(Native Method)
    09-05 14:40:02.497: E/AndroidRuntime(823): at java.lang.Class.newInstance(Class.java:1319)
    09-05 14:40:02.497: E/AndroidRuntime(823): at android.app.Instrumentation.newActivity(Instrumentation.java:1023)
    09-05 14:40:02.497: E/AndroidRuntime(823): at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:1870)

## Implement a Fix
You will want to double check how you initialize your DataSource class, here is it called Database. Originally, `getBaseContext()` was called to fulfill the argument requirements but it was causing the `Null Pointer Exception`. 

Once we updated it to the below, everything started working:

    database = new Database(getApplicationContext());
