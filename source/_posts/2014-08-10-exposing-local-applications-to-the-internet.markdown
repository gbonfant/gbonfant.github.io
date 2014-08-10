---
layout: post
title: "Exposing local applications to the internet"
date: 2014-08-10 21:02:25 +0200
comments: true
description: "Exposing local applications to the internet with ngrok"
categories: productivity
---

In a [recent post](http://www.gbonfant.com/blog/embracing-pair-programming-tmux-pow/) where I discussed my current environment for remote pair programming I mentioned the use of [pow!](http://pow.cx/) for sharing local rake applications with my remote pair, or even local coworkers. However, this setup only worked for rake applications. What then if you are running a node/python/scala server?

Recently I stumbled upon [ngrok](ngrok.com), and it has changed my development cycle for the better. Ngrok is a superpowered pow that works with any application and it comes with all kind of nifty features.

<!-- more -->

In a nutshell ngrok uses [tunneling](http://en.wikipedia.org/wiki/Tunneling_protocol) to tunnel traffic from a public domain such as ``http://subdomain.ngrok.com`` to a port on your local machine. The coolest thing of this though, is that it captures all the traffic that is passing through the tunnel, allowing you to inspect any kind of transaction.

Now you might think you can accomplish the same thing by tailing your logs and, if you have properly configured logging, you would be right. However, ngrok takes this further by allowing you to replay any previous transactions from a simple but powerful UI, it looks like this:

{% img /images/ngrok-ui.jpg 'ngrok UI' %}

So not only you can share your local application with your remote pair, you get to see a clear overview of all the incoming requests, along with pretty-printed versions of any JSON/XML transactions, and you get to easily replicate bugs by replaying a request! This is specially powerful for testing remote APIs or inspecting a tester's scenarios.

Ngrok is super simple to setup, simply run ``$ brew install ngrok`` if you are on OS X or download the single binary at its homepage.
