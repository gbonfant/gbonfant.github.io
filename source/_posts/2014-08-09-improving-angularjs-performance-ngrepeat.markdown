---
layout: post
title: "Improving AngularJS performance: ngRepeat"
date: 2014-08-09 23:28:31 +0200
comments: true
categories: javascript, angularjs, performance
---

When looking into debugging JavaScript performance bottlenecks memory leaks are one of the initial suspects. Most of the time these occur due to objects not being swept by the garbage collector, the most common scenario is deleting node elements that have event listeners attached to them.

However, when looking at AngularJS applications, is easier to begging by looking at the ngRepeat directive. DOM operations are expensive and ngRepeat is not particularly efficient at them, by default the directive will create as many DOM nodes as items in your collection and will destroy and recreate these nodes every time the underlying data changes, furthermore ngRepeat will mutate your original collection in order to associate your JavaScript object to a DOM node.

These kind of mutability can be seen when creating custom directives, if you inspect the scope of the directive you'll see a ``$$hashKey`` attribute appended. So, how can we avoid ngRepeat from recreating DOM nodes and mutating our objects?

## Enter track by clause

Introduced in Angular 1.2, ``track by`` allows you to specify your own identity key. This will not only cause ngRepeat from injecting ``$$hashkey`` but it will **not** recreate DOM nodes for which their underlying associated objects have not changed. This also saves any linking or compiling phases on your directives from being executed.

I have created a [fiddle]() to showcase this behaviour. In this scenario we simply create a collection with 300 objects on it and rebuild it every time we click a button. As we can see below on every rebuild we are creating more nodes and every rebuild operation takes aproximately 100~200ms


On the other hand, if we provide a unique, immutable id to the ngRepeat directive we can see how the node count does not increase and how every rebuild operation takes only ~6ms to run.
