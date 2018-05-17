---
templateKey: blog-post
title: AngularJS Detect Enter Key - ngEnter
date: '2014-03-14T10:29:00-07:00'
tags:
  - angularjs
  - angularjs detect enter
  - angularjs enter
  - angularjs example
  - angularjs ngenter
---
AngularJS has a ton of built in directives and functions to make your app come alive very quickly.  One thing I noticed it was lacking was a built in on-enter event.  For example, if a user is in an input field when they hit enter I want it to do something, maybe not necessarily submit the form.  Luckily, AngularJS gives you the ability to create your own directives and add them into your app.  Below is a simple directive that you can add to your app that allows you to add <code>ng-enter</code> to elements in your partials.  Pass it a function and watch as you hit enter and that function is executed flawlessly.
<pre class="lang:default decode:true">/*
This directive allows us to pass a function in on an enter key to do what we want.
 */
app.directive('ngEnter', function () {
    return function (scope, element, attrs) {
        element.bind("keydown keypress", function (event) {
            if(event.which === 13) {
                scope.$apply(function (){
                    scope.$eval(attrs.ngEnter);
                });

                event.preventDefault();
            }
        });
    };
});</pre>
That's it.  Now just add <code>ng-enter="myFunction()"</code> to any element in your partial that detects keystrokes. This has helped me a ton and added a lot of easy functionality to an already great AngularJS system.  If you have any other great directives or AngularJS tips please leave them below in the comments.
