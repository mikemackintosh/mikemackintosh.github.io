---
layout: post
permalink: /highcharts-x-axis-timestamps-off
title: "Highcharts x-Axis Timestamp Labels Not Matching Data"
category: "One-Liner"
tags: datetime highcharts highstock timestamp use-utc utc x-axis javascript jquery
---
### Introduction

I kept running into an issue where I was extracting data from a database and loading it into HighCharts/HighStock. This was a great, fast and simple solution to graphically represent the statistics of some of my systems. Unfortunately, when I broke the stats down by 5-minute intervals, I noticed that the irregular datetime labels on the x-Axis were off by about 5 hours. After looking around Google for quite some time, I stumbled upon the following fix: `useUTC: false`.

## The Fix

To fix this issue, place the following above your highcharts javascript: 

```
Highcharts.setOptions({ global: { useUTC: false } });
```