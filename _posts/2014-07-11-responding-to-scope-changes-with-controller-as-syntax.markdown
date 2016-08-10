---
layout: post
title: "Responding to scope changes with controller as syntax"
date: 2014-07-11 14:51:50 +0200
category: blog
description: "How to use $watch expressions with controllerAs syntax."
---

Before the release of Angular 1.2 data models were usually binded to the ``$scope`` object in a controller, I find this pattern confusing as the meaning of scope across angular applications is not consistent, nor clear for many beginners.

Starting on [1.1.5](https://github.com/angular/angular.js/blob/master/CHANGELOG.md#115-triangle-squarification-2013-05-22) the *controllerAs* syntax was introduced. Essentially allowing the use of Plain Old JavaScript Objects[^1] to define controllers.

~~~ javascript
app.controller('LoginController', function() {
  this.username = 'John';
});
~~~

Although I very much like this pattern, at the moment of its release it introduced a regression in my code base: manually using $watch on object references stopped working.

~~~ javascript
app.controller('LoginController', function($scope) {
  this.username = 'John';

  $scope.$watch('username', function(newVal, oldVal) {
    // does not work
  });
});
~~~

It turns out, the first argument we pass to $watch, or *watchExpression*, is a simple function that returns the value to watch for in the next $digest cycle, meaning we can then pass a function instead of the usual string in order to regain the lost functionality:


~~~ javascript
app.controller('LoginController', function($scope) {
  this.username = 'John';

  $scope.$watch(
    angular.bind(this, function() { return this.username; }),
    function(newVal, oldVal) {
      // does work!
    }
  );
});
~~~

[^1]: [http://odetocode.com/blogs/scott/archive/2012/02/27/plain-old-javascript.aspx](http://odetocode.com/blogs/scott/archive/2012/02/27/plain-old-javascript.aspx)
