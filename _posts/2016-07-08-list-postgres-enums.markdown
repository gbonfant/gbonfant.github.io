---
layout: post
title: "List all defined ENUM in Postgres"
date: 2016-07-08 11:00:00 +0000
description: "List all defined ENUM in Postgres"
category: blog
---

Enumerated is a Postgres data type comprised by ordered, static values. The same concept can be found in mathematics, and many programming languages. For instance, an enumerable in ruby can be defined as a collection of symbols.

{% highlight ruby %}
enums = [:on, :off, :standby]
{% endhighlight %}

These are useful for a variety of things, such as restricting database values to a predefined set. Querying for all defined enums in postgres can be a bit non-intuitive, the following query should take care of that:

{% highlight sql %}
SELECT pg_type.typname AS enum_type, pg_enum.enumlabel AS enmu_label FROM pg_type JOIN pg_enum ON pg_enum.enumtypid = pg_type.oid;
{% endhighlight %}
