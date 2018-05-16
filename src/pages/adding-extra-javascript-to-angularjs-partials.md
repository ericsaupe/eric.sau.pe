---
templateKey: blog-post
title: Adding Extra JavaScript to AngularJS Partials
date: '2014-01-24T15:30:00-08:00'
tags:
  - angularjs
  - angularjs jquery
  - angularjs load javascript partials
  - angularjs script partial
  - angularjs script partials
---
When loading partials into your one page AngularJS app occasionally you'll need to load in JavaScript files that pertain only to those partials.  If you are only using AngularJS these added &lt;script&gt; tags won't be rendered by Angular.  After Googling for some time I have found that the solution is to load JQuery into your index.html file just before AngularJS.  This fixes the problem.

<strong>partial.html</strong>
<pre class="theme:twilight toolbar:2 lang:js decode:true" title="partial.html">&lt;script type="text/javascript" src="//cdn.someplace.com/file.js/"&gt;&lt;/script&gt;</pre>
<strong>index.html</strong>
<pre class="theme:twilight toolbar:2 lang:js decode:true" title="index.html">&lt;script type="text/javascript" src="//code.jquery.com/ui/1.10.3/jquery-ui.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.0.1/angular.min.js"&gt;&lt;/script&gt;</pre>
&nbsp;

Problem solved. Hope this helps someone else who runs into the same problem I had.  If there is a better, more Angular way, of doing this let me know in the comments below.
