---
layout: post
title: "Vectra Networks Dashing Widget"
category: security
tags: vectra network api ruby dashing
---

For those who know me, know I've been working with a new vendor, Vectra Networks. These guys have one of the most solid and responsive systems I've seen yet when it comes to malware mapping and threat networking and detection.

Being a one-man security team, I need to come up with a way to monitor a million tools at once; enter [Dashing](http://dashing.io/).

{% gist mikemackintosh/8edc444764089391c001 README.md %}

Using the API that Vectras' appliance provides, we can easily pull a lot of the information that their own dashboard normally provides. To achieve the API connectivity, I created this [Vectra Ruby API Client](https://github.com/mikemackintosh/ruby-vectra).

## The Scheduler Job

{% gist mikemackintosh/8edc444764089391c001 vectra.rb %}

{% gist mikemackintosh/8edc444764089391c001 DEPENDENCIES.md %}
