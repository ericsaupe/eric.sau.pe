---
templateKey: blog-post
title: Using Twitter Typeahead.js Custom Event Triggers
date: '2013-10-29T15:05:00-07:00'
tags:
  - twitter typeahead
  - twitter typeahead.js
  - twitter typeahead.js custom events
  - twitter typeahead.js custom triggers
  - typeahead.js
  - typeahead.js custom events
---
Since Bootstrap dropped support for their typeahead they have since told people to use Twitter's typeahead.js.  It has a lot of great features that I have covered in an older write up about <a title="Twitter Typeahead.js" href="http://ericsaupe.com/twitter-typeahead-js/" target="_blank">Twitter Typeahead.js</a>.  In this post I wanted to go over the custom events that are triggered and how you can use them to get more out of Twitter Typehead.

There are five triggers in the typeahead.js and you can tap into each of these to do what you'd like when these events are triggered.  Below are those events and when they are triggered.
<ul>
	<li><code>typeahead:initialized</code> – Triggered after initialization. If data needs to be prefetched, this event will not be triggered until after the prefetched data is processed.</li>
	<li><code>typeahead:opened</code> – Triggered when the dropdown menu of a typeahead is opened.</li>
	<li><code>typeahead:closed</code> – Triggered when the dropdown menu of a typeahead is closed.</li>
	<li><code>typeahead:selected</code> – <del>Triggered when a suggestion from the dropdown menu is explicitly selected. The datum for the selected suggestion is passed to the event handler as an argument in addition to the name of the dataset it originated from.</del>  Triggered when the user hits Tab to complete the text provided in the text box.  See comments section below.</li>
	<li><code>typeahead:autocompleted</code> – <del>Triggered when the query is autocompleted. The datum used for autocompletion is passed to the event handler as an argument in addition to the name of the dataset it originated from.</del>  Triggered when the user clicks, or uses the arrow keys, to select an element from the drop down.  See comments section below.</li>
</ul>
All of that information comes directly from the <a href="https://github.com/twitter/typeahead.js/blob/master/README.md#custom-events">typeahead README</a>.  Now that we know what the events are and when they are triggered we can make custom events that happen by using jQuery and tapping into those events using .on().  Say, for example, I wanted to update a title on a page including the input field that typeahead already does.  You could make an on change event that would listen on that input for a change and then alter the title but in our case we can simplify that by tapping into the typeahead:selected event.
<pre class="lang:js decode:true">$('.typeahead')
    .typeahead(/* pass in your datasets and initialize the typeahead */)
    .on('typeahead:selected', function($e, datum){
        $("#title").html(datum["value"]);
        }
    );</pre>
Now instead of a second listener on your input object you can just have Twitter Typeahead listen for you and make changes where you need.  Play around with the different variations of the events to help you get the most out of Twitter Typeahead.  There is an issue on their GitHub page that explains how to use the custom events and gives a very basic example.  Below is the example and here is the link to <a href="https://github.com/twitter/typeahead.js/issues/163">Getting a custom event to fire</a>.
<pre class="lang:js decode:true">$('.typeahead')
    .typeahead(/* pass in your datasets and initialize the typeahead */)
    .on('typeahead:opened', onOpened)
    .on('typeahead:selected', onAutocompleted)
    .on('typeahead:autocompleted', onSelected);

function onOpened($e) {
    console.log('opened');
}

function onAutocompleted($e, datum) {
    console.log('autocompleted');
    console.log(datum);
}

function onSelected($e, datum) {
    console.log('selected');
    console.log(datum);
}</pre>
Hope that helps someone as it took me a bit of digging to find the easiest way to use those triggers.  Comment below on other useful ways you've used Twitter Typeahead and the custom event triggers.
