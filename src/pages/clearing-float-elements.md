---
templateKey: blog-post
title: Clearing Float Elements
date: '2013-10-24T15:03:00-07:00'
tags:
  - css
  - css float
  - css float fix
  - float
  - html
---
As you develop web pages you may come across moments when you need to float an element.  Using the CSS float you can make elements be pushed around to make room as more elements are added.    A nifty feature in and of itself but there is a common problem that tends to crop up.

Recently I was working on a small navigation header that included two buttons and a search box.  The buttons were standard <code>button</code> elements and the search box had been floated to move with the varying size of the screen.  The problem that happened came when you made the screen too small and the floated box dropped to the next line.  Directly beneath this row of elements was a header row that had a background and when the search box dropped down it would be hidden by this header instead of pushing the header down as would be expected.

After doing some research into float elements I came across this question on StackOverflow asking <a href="http://stackoverflow.com/q/490184/575985" target="_blank">What is the best way to clear the CSS style "float"?</a>  In it the top answer explained that you can add an arbitrary <code>br</code> element with <code>clear:both</code> as the style to force floated elements down a line but that isn't ideal.  Their solution was to wrap your float elements in a parent div and simply give that div the CSS of <code>overflow: hidden</code>.  This forces all floated elements to stay within the parent div.  With this parent and CSS in place the parent div will grow depending on what happens within, floated element or not.
<pre class="lang:default decode:true ">&lt;form&gt;
    &lt;input type="text" style="float: left;"&gt;
    &lt;button type="button"&gt;Refresh&lt;/button&gt;
    &lt;button type="button"&gt;Close&lt;/button&gt;
&lt;/form&gt;</pre>
Becomes
<pre class="lang:default decode:true ">&lt;div style="overflow: hidden"&gt;
    &lt;form&gt;
        &lt;input type="text" style="float: left;"&gt;
        &lt;button type="button"&gt;Refresh&lt;/button&gt;
        &lt;button type="button"&gt;Close&lt;/button&gt;
    &lt;/form&gt;
&lt;/div&gt;</pre>
This solved my problem perfectly and works cross browser so feel free to use it everywhere and anywhere.  Hope this helps someone else out there who is having similar problems.  Float elements can be very tricky sometimes.
