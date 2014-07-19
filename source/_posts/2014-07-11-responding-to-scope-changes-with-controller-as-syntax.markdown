---
layout: post
title: "Responding to scope changes with controller as syntax"
date: 2014-07-11 14:51:50 +0200
comments: true
description: "Responding to scope changes with controller as syntax in AngularJS"
categories: javascript angularjs
---

Before AngularJS 1.2 we used to bind our data models to the ``$scope`` object in a controller. I find this confusing as the use  of ``scope`` across my angular code looked like some dark magic.

Starting on [1.1.5](https://github.com/angular/angular.js/blob/master/CHANGELOG.md#115-triangle-squarification-2013-05-22) the *controller as* syntax was introduced, this allowed us to declare our controllers as plain javascript objects, such as:

```javascript
myApp.controller('LoginController', function() {
  this.username = 'James';
});
```

I liked this approach but it introduced an annoyance: watching scope changes didn't work anymore.

<!-- more -->

```javascript
myApp.controller('LoginController', ['$scope', 'NotificationFactory', function($scope, NotificationFactory) {
  this.name          = 'James';
  this.notifications = NotificationFactory;

  $scope.watch('notifications', function(newValue, oldValue) {
    console.log('This does not work');
  });
}]);
```

It turns out you can pass a function as a listener argument to ``$watch`` instead of a simple string, meaning that, after properly binding our current context, we could watch our controller's object like so:

```javascript
myApp.controller('LoginController', ['$scope', 'NotificationFactory', function($scope, NotificationFactory) {
  this.name          = 'James';
  this.notifications = NotificationFactory;

  $scope.watch(
    angular.bind(this, function() { return this.notifications.errors; }),

    function(newValue, oldValue) {
      // This does work!
      console.log(newValue, oldValue);
    }
  );
}]);
```
