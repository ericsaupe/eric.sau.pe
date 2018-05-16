---
templateKey: blog-post
title: Getting Started With jQuery
date: '2013-11-05T15:06:00-08:00'
tags:
  - jquery
  - jquery document ready
  - jquery example
  - jquery ready
  - jquery setup
  - load jquery
---
I write a lot of posts that use <a href="http://jquery.com/">jQuery</a> but I haven't gone through the basics of getting started with jQuery on your new website.  There are just a couple of steps that you need to do to get started with using jQuery and getting it started right.

The first thing you'll need to do is load jQuery into your website.  The easiest way to do that is to add the following line inside the <code>&lt;body&gt;</code> of your base HTML template that will be used throughout the site.

<pre>&lt;script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"&gt;&lt;/script&gt;</pre>

Google hosts a number of <a href="https://developers.google.com/speed/libraries/devguide" target="_blank">web libraries</a> and they are a great way to get the latest version of various libraries without have to hit your servers with serving those same files.  If you are using an older version, or an altered version, of jQuery you'll want to host it yourself but this is the simplest way to get started.

Once that has been loaded into the <code>&lt;body&gt;</code> of your base HTML file you can start writing JavaScript using jQuery.  The best way to ensure that your code is run in a timely fashion, namely after the page has been completely loaded, you'll want to run document.ready.  The jQuery document.ready is run once everything has been loaded into the DOM, in other words once everything, all the inputs and CSS, has been loaded in this function will run.  It's best to put a lot of your code within this function to ensure that everything that the triggers will be applied to are loaded and can have these triggers applied.  Your JavaScript file should look something like this.
<pre class="lang:js decode:true ">$(document).ready(function(){
    alert("Page has been loaded.");
}</pre>
Feel free to put whatever you want inside of that ready function.  You can even make calls to other JavaScript functions you have in the file.  I hope this helps people get started with jQuery.  I've seen a lot of people struggle with getting it imported into their site and have it run smoothly.  If you have any tips or questions about jQuery feel free to leave a comment below.
