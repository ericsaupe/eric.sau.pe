---
templateKey: blog-post
title: AngularFire Filtering Data Returned by Firebase
date: '2014-03-04T10:27:00-08:00'
tags:
  - angularfire
  - angularfire orderByPriority
  - angularjs
  - angularJS orderByPriority
  - firebase
  - firebase orderByPriority
  - orderByPriority
---
<a title="Firebase" href="https://www.firebase.com/" target="_blank">Firebase</a> is a great way to connect applications to a database that will sync data in realtime.  Tying this in with <a title="AngularJS" href="http://angularjs.org/" target="_blank">AngularJS</a> allows you to create a single page application that updates dynamically and as data changes without page refreshes.  This allows you to create applications that can give your users access to data the moment it is updated and changed.  They have a great binding called <a title="AngularFire" href="https://www.firebase.com/quickstart/angularjs.html" target="_blank">AngularFire</a> that links the two services together nicely.

One thing that I had problems with was filtering data returned by Firebase with the great AngularJS filter method.  Because AngularFire returns a dictionary with methods and elements the AngularJS filter has problems filtering the data correctly.  The AngularJS template rendering can render the data just fine but the filter does not work.  To fix that you'll need to employ the AngularFire method <a title="orderByPriority" href="https://www.firebase.com/docs/angular/reference.html#orderbypriority" target="_blank">orderByPriority</a> before your AngularJS filter.  Here is an example.
<pre class="lang:default decode:true">&lt;input type="text" class="form-control" ng-model="storeSearch.$" placeholder="Store Search"&gt;
&lt;ul class="list-unstyled" ng-repeat="store in stores | orderByPriority | filter:storeSearch"&gt;
	&lt;li&gt;&lt;a href="#/stores/{{store.name}}/" class="btn btn-danger btn-lg"&gt;{{store.name}}&lt;br/&gt;{{store.address}}&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;

&lt;script&gt;
angular.module('myApp.controllers', [])
.controller('StoresController', ['$scope', '$firebase', function($scope, $firebase) {
    var ref = new Firebase('https://MYFIREBASE.firebaseio.com/stores/');
  	$scope.stores = $firebase(ref);
  }])
&lt;/script&gt;</pre>
In my Controller I access the list of stores using Firebase and $firebase.  At this point my $scope has the list of stores with some methods used by AngularFire to update data.  The partial above will render the list just fine but without orderByPriority the search filter would not work.  The orderByPriority converts an object returned by $firebase into an array.  This array will work with any normal AnguarJS filter.

Here is the definition from the API of AngularFire.
<blockquote>The <code>orderByPriority</code> filter is provided by AngularFire to convert an object returned by <code>$firebase</code> into an array. The objects in the array are ordered by priority (as defined in Firebase). Additionally, each object in the array will have a<code>$id</code> property defined on it, which will correspond to the key name for that object.</blockquote>
That's all there is to it.  I hope this helps someone else figure out this problem as I spent a long time trying to get my filter to work with Firebase objects.
