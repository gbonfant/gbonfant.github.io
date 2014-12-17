---
layout: post-no-feature
title: "Failing integration tests on JavaScript-rich Rails applications"
date: 2014-08-18 16:58:55 +0200
category: blog
description: "How to fix debugging errors for JavaScript in Capybara tests"
---

In a recent project I found myself with a failing integration test for a part of the application in where a JavaScript MVC framework was being used. The output wasn't particularly helpful:

{% highlight js linenos %}
TypeError: 'undefined' is not a function (near '...}.bind(this),...')
    at FooController (http://127.0.0.1:59119/assets/application.js:64502)
{% endhighlight %}

This is caused because Rails serves a compressed version of your assets in the test environment. The simplest way to solve this is to update the environment files

{% highlight ruby linenos %}
# config/environments/test.rb
config.assets.debug = true
{% endhighlight %}

Now errors are easier to track:

{% highlight js linenos %}
TypeError: 'undefined' is not a function (near '...}.bind(this),...')
    at FooController (http://127.0.0.1:59323/assets/angular/controllers/foo_controller.js?body=1:72)
{% endhighlight %}

Happy debugging!
