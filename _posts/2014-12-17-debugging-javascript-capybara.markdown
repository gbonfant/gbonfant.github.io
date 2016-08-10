---
layout: post
title: "Better JavaScript errors in Capybara tests"
date: 2014-12-17 13:58:00 +0000
category: blog
description: "How to fix debugging errors for JavaScript in Capybara tests"
---

In a recent project I found myself with a failing integration test for a part of the application in where a JavaScript MVC framework was being used.

The output wasn't particularly helpful:

~~~ js
TypeError: 'undefined' is not a function (near '...}.bind(this),...')
    at TrolleyController (http://127.0.0.1:59119/assets/application.js:64502)
~~~

This is caused because Rails serves a compressed version of your assets in the test environment. The simplest way to solve this is to update the environment files

~~~ ruby
# config/environments/test.rb
config.assets.debug = true
~~~

Now errors are easier to track:

~~~ js
TypeError: 'undefined' is not a function (near '...}.bind(this),...')
    at TrolleyController (http://127.0.0.1:59323/assets/angular/trolley/trolley_controller.js?body=1:72)
~~~

Happy debugging!
