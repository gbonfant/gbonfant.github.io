---
layout: post
title: "Execute a function on ng-repeat done"
date: 2013-08-25 21:58:05 +0200
comments: true
description: "How to execute a function on ng-repeat done Angular.js"
categories: angularjs javascript
---

ngRepeat is one of Angular's most powerful directives and in most cases _it just works_, however there are times when I find myself wanting to update the DOM or execute a function as soon as the directive has finished its job.

Thanks to Angular's [$destroy()](http://docs.angularjs.org/api/ng.$rootScope.Scope#$destroy) I can create a custom directive to accomplish this.

```javascript
myApp.directive('repeatDone', function() {
  return function(scope, element, attrs) {
    element.bind('$destroy', function(event) {
      if (scope.$last) {
        scope.$eval(attrs.repeatDone);
      }
    });
  };
});
```

By calling my new directive along with ``ng-repeat`` I can execute a function as soon as the iteration is finished.

```html
<div ng-repeat="item in cart.items" repeat-done="foo()"></div>
```
