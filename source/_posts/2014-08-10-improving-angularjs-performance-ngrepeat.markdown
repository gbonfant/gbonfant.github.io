---
layout: post
title: "Improving AngularJS performance: ngRepeat"
date: 2014-08-10 09:28:31 +0200
comments: false
categories: javascript, angularjs, performance
---

When looking into debugging JavaScript performance bottlenecks memory leaks are one of the initial suspects. Most of the time these occur due to dead objects not being cleaned by the garbage collector, the most common scenario is keeping a reference somewhere in your code to node elements with event listeners that have been deleted.

However, when looking at performance bottlenecks in AngularJS applications, I find it easier to begging by inspecting the usage of ngRepeat. DOM operations are expensive and ngRepeat is not particularly efficient at them, by default the directive will create as many DOM nodes as items in your collection, and subsequently destroy and recreate these nodes every time the underlying data changes, furthermore ngRepeat will mutate your original collection in order to associate your JavaScript objects to a DOM node.

This kind of inadvertent mutability can be spotted when working with custom directives, by inspecting the scope of the directive we can see a ``$$hashKey`` attribute appended. So, how can we avoid ngRepeat from recreating DOM nodes and mutating our objects?

## Enter track by clause

Introduced in Angular 1.2, ``track by`` allows us to specify an identity key, preventing ngRepeat from injecting ``$$hashkey``. Most importantly the directive will **not** recreate DOM nodes for which their underlying associated objects have not changed. This also keeps any unnecessary linking or compiling phases from being executed.

I have created a [fiddle](http://jsfiddle.net/gbonfant/Mrn66/6/) to showcase this behaviour. In this scenario we simply create a collection with 300 objects and rebuild it every time we click a button.


As we can see on every rebuild we are creating more nodes and every operation takes approximately 100~200ms.

On the other hand, if we provide a unique, immutable id to the ngRepeat directive the node count does not increase and every rebuild operation takes only ~6ms.

## Conclusion

Whenever possible provide a unique, immutable identity key to ngRepeat and keep collections small.
