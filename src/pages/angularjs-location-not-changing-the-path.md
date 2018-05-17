---
templateKey: blog-post
title: AngularJS $location Not Changing the Path
date: '2014-02-22T10:26:00-08:00'
tags:
  - angularfire
  - angularfire login
  - angularjs
  - angularjs $location
  - AngularJS $location Not Changing the Path
  - angularjs $location.path
  - angularjs firebase
  - angularjs firebase firebasesimplelogin
  - angularjs firebase login
  - firebase simple login
  - firebase simplelogin
  - firebasesimplelogin
---
In my adventures learning AngularJS I am confronted with strange problems all the time which is a really great thing.  I love learning how to do new and intersting things in different programming environments.  Today I was having problems with my URL not forwarding correctly on a callback method.  Once a user was logged in I wanted the URL to forward to another page.  I was using <code>$location.path</code> and setting it to a URL that was defined in my <code>$routeProvider</code> but it wasn't actually going to that page.

A quick Google search led me to this Stack Overflow page, <a title="AngularJS $location not changing the path" href="http://stackoverflow.com/q/11784656/575985" target="_blank">AngularJS $location not changing the path</a>, with the exact problem I was having.  The answer, it turns out, is to add <code>$scope.apply()</code> after any changes you've made.  This does a refresh on the scope and applies any changes you've made that may not have been updated.  The answer describes a conflict with another library in his case jQuery.  I am using Firebase to log the user in and I believe the conflict is there.

Here is my code and a little bit of explanation with it.
<h4>controllers.js</h4>
<pre class="lang:js decode:true">.controller('AuthController', ['$scope', '$rootScope', '$location', 'AuthService', function($scope, $rootScope, $location, AuthService) {
    $scope.login = function(email, password) {
      var ref = new Firebase('https://myfirebaseapp.firebaseio.com');
      var auth = new FirebaseSimpleLogin(ref, function(error, user) {
        if (error) {
          // an error occurred while attempting login
          console.log(error);
        } else if (user) {
          // user authenticated with Firebase
          $rootScope.user = user;
          $location.path('/stores').replace();
          $scope.$apply();
        } else {
          // user is logged out
        }
      });
      AuthService.login(email, password, auth);
    }
    $scope.createUser = function(email, password) {
      var ref = new Firebase('https://myfirebaseapp.firebaseio.com');
      var auth = new FirebaseSimpleLogin(ref, function(error, user) {
        if (error) {
          // an error occurred while attempting login
          console.log(error);
        } else if (user) {
          // user authenticated with Firebase
          $rootScope.user = user;
          $location.path('/stores').replace();
          $scope.$apply();
        } else {
          // user is logged out
        }
      });
      AuthService.createUser(email, password, auth);
    }
    $scope.logout = function() {
      var ref = new Firebase('https://myfirebaseapp.firebaseio.com');
      var auth = new FirebaseSimpleLogin(ref, function(error, user) {
        if (error) {
          // an error occurred while attempting login
          console.log(error);
        } else if (user) {
          // user authenticated with Firebase
        } else {
          // user is logged out
          $rootScope.user = null;
        }
      });
      AuthService.logout(auth);
    }
  }])</pre>
<h4>services.js</h4>
<pre class="lang:js decode:true">.factory('AuthService', ['$rootScope', '$location', function($rootScope, $location) {
    return {
	    createUser: function(email, password, auth) {
		    auth.logout();
	      auth.createUser(email, password, function(error, user) {
	        if (!error) {
	          $rootScope.user = user;
	        }
	      });
	    },
	    login: function(email, password, auth) {
		    auth.logout();
	      auth.login('password', {
	        email: email,
	        password: password,
	        rememberMe: false
	      });
	    },
	    logout: function(auth) {
	    	auth.logout();
	    }
  	}
  }]);</pre>
The controllers.js file contains a simple controller to put my login, logout, and createUser functions in.  I have off loaded the actual Firebase login to a service that I load in.  I had a really hard time getting it all to work and found that putting the Firebase connections into the controller method and passing that to the service was the best route.  I had the Firebase connection in the service but the callbacks were not firing quite in sync with the controller.  This way I was able to get a more consistent timing of events.

The FirebaseSimpleLogin has a callback that gets fired whenever an event happens with it.  In our case that is user is created, user logs in, or user logs out.  I then save the user data to the <code>$rootScope</code> to access it throughout my application and change the URL to a path of my choosing.  At this point without the <code>$scope.$apply()</code> the URL would never actually get fired nor would the $scope realize that there was now a user object, though with the Chrome AngularJS plugin I could clearly see the data was in the scope.  Using the <code>$scope.$apply()</code> cleared all of this up and it works great now.

If anyone has any comments on this or any bit of my code above I am very open to hearing about it.  I am still learning AngularJS and am still unsure of proper coding standards.
