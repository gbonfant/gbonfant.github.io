---
layout: post-no-feature
title: "Exposing local applications to the internet"
date: 2014-08-10 21:02:25 +0200
category: blog
description: "Easily sharing local applications with ngrok."
---

In a [recent post](http://www.gbonfant.com/blog/embracing-pair-programming-tmux-pow/) I mentioned how I used [pow!](http://pow.cx/) to share my running rake applications with my remote pair, or even local coworkers. However, that setup only worked for rake applications. What then if you are running a node/python/scala server?

Until recently I simply screen shared my logs or browser window, that is until I stumbled upon [ngrok](http://ngrok.com). Ever since my workflow has dramatically changed.

Ngrok is a superpowered pow that works with any application and it comes with all kind of nifty features. Ngrok uses [tunneling](http://en.wikipedia.org/wiki/Tunneling_protocol) to tunnel traffic from a public domain such as ``http://subdomain.ngrok.com`` to a port on your local machine. Furthermore all transactions are captured and neatly exposed through a web UI for detailed inspection. An image is worth a thousand words:

![Ngrok web UI](/images/ngrok-ui.jpg)

Ngrok is super simple to setup, if you are on a mac simply run

{% highlight bash %}
$ brew install ngrok
{% endhighlight %}

Otherwise [download](https://ngrok.com/download) the single binary from their site.
