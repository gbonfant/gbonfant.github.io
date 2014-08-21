---
layout: post-no-feature
title: "Improving AngularJS performance: ngRepeat"
date: 2014-08-10 09:28:31 +0200
category: blog
description: "ng-repeat can become the main bottleneck of your application. Here's how to prevent that."
---

Memory leaks are oftentimes the main culprits of sluggishness in rich JavaScript applications. Most of the time these leaks occur due to trashed objects not being cleaned by the garbage collector. The most common scenario is keeping a reference, somewhere in your code, to node elements with event listeners that have been deleted.

However, in AngularJS applications, ngRepeat is often the reason for jerkiness and other breakages. In web development it is well known that DOM operations are expensive. Out of the box ngRepeat is not particularly efficient nor it tries to reduce DOM manipulation. By default the directive will create as many DOM nodes as there are items in your collection, subsequently it'll destroy and recreate these nodes every time the underlying data changes. To make things worse ngRepeat will mutate your original collection in order to associate your JavaScript objects to a DOM node.

You might have come across this kind of inadvertent mutability if you've ever written a custom directive, that is the dreaded ``$$hashKey`` attribute – which appends itself to every object in the collection. So, how can we avoid ngRepeat from recreating DOM nodes and mutating objects?

### Enter track by clause

Introduced in Angular 1.2, ``track by`` allows us to specify an identity key, preventing ngRepeat from injecting ``$$hashKey``. Most importantly the directive will **not** recreate DOM nodes for which their underlying associated objects have not changed. This also keeps any unnecessary linking or compiling phases from being executed.

I have created a [fiddle](http://jsfiddle.net/gbonfant/Mrn66/6/) to showcase this behaviour.

In the first scenario I simply create a collection with 300 objects and rebuild it every time a button is clicked.

![ngRepeat without track by - memory usage graph](/images/ngRepeat-memory.jpg)

![ngRepeat without track by - stack chart](/images/ngRepeat-stack.jpg)

By inspecting the graph is evident that more and more nodes are created, also every operation takes approximately _100~200ms_.

On the other hand, by providing a unique, immutable id to the ngRepeat directive the node count does not increase and every rebuild operation takes only _~6ms_.

![ngRepeat with track by - memory usage graph](/images/ngRepeat-memory-trackby.jpg)

![ngRepeat with track by - stack chart](/images/ngRepeat-stack-trackby.jpg)

### Conclusion

Whenever possible provide a unique, immutable identity key to ngRepeat, keep collections small, and avoid unnecessary DOM operations.
