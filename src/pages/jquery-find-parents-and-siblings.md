---
templateKey: blog-post
title: 'JQuery Find, Parents, and Siblings'
date: '2013-10-15T14:59:00-07:00'
tags:
  - jquery
  - jquery children
  - jquery find
  - jquery parent
  - jquery parents
  - jquery siblings
  - jquery traversal
---
A very important tool when working with JQuery is the ability to traverse through elements in the DOM.  When a user clicks on a button you may want to change the color of the nearest row to display the click.  Maybe after an input you want to change the nearest column to be hidden or show that input value.  Whenever you need to walk through the elements in your page JQuery has a way to do it.  All of the methods shown here can be called with no arguments but are more helpful if you call them with a selector of some kind either an id, class, or element type.

The first question you have to ask yourself is where is the element I'm looking for from my current location?  For instance, that button that was clicked is on a row.  The row element contains the button so we need to travel upwards.  When traveling up you can do one of two things, <a href="http://api.jquery.com/parent/"><code>$.parent()</code></a> or <a href="http://api.jquery.com/parents/"><code>$.parents()</code></a>.  The difference between the two is that the latter travels more than just a single level in the DOM.  Obviously that makes it a bit less efficient but it is exhaustive.  If you know you'll need to travel up a layer or two to find the parent element you're looking for then use parents.  The example below demonstrates the button changing the parent row it is in.

<code>$( "button" ).parents("tr").css("font-family", "Times New Roman" );</code>

If you need to go down in the DOM you can use either <a href="http://api.jquery.com/children/"><code>$.children()</code></a> or <a href="http://api.jquery.com/find/"><code>$.find()</code></a>.  Again the difference is that the latter travels more than one layer.  An interesting thing that may happen is when you are at the level you want to be but need to find an element at that same level.  That is where <a href="http://api.jquery.com/siblings/"><code>$.siblings()</code></a> comes in.  It will search for the element on the current level of the DOM you are in.  Combining these methods together can help you locate elements all throughout your page.  For example, once I needed to find the nearest error messages div because I had one near a variety of inputs and place text inside of it.  I first used <code>$.parents('error-row')</code> to find the row it was in.  Then used <code>$.children('.error-display')</code> to find the div within that held the text.

Obviously naming things uniquely with id's is the safest way to go as you can reference things directly but in the case of a complicated page with many layers and hidden elements it may be nice to traverse your page to find your elements.  It also may be easier to simply change the nearest element rather than name every little item on your page.  JQuery traversal has its place and remember if you can't find it at one level you may need to go up one more.
