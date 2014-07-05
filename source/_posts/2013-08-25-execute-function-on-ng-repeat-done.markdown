---
layout: post
title: "Execute a function on ng-repeat done"
date: 2013-08-25 21:58:05 +0200
comments: true
description: "How to execute a function on ng-repeat done Angular.js"
categories:
---

ngRepeat is one of Angular's most powerful directives, however in certain cases you might find yourself wanting to update the DOM or execute a function as soon as the directive has finished its job.

Along with [$destroy()](http://docs.angularjs.org/api/ng.$rootScope.Scope#$destroy) we can create a custom directive to accomplish such effect.

```javascript
var app = angular.module("app", []);

app.directive('repeatDone', function() {
  return function(scope, element, attrs) {
    element.bind('$destroy', function(event) {
      if (scope.$last) {
        scope.$eval(attrs.repeatDone);
      }
    });
  };
});
```
We can then simply use it along ngRepeat in the following fashion:

```html
<div ng-repeat="item in model.items" repeat-done="foo()"></div>
```
