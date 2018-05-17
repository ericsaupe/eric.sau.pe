---
templateKey: blog-post
title: Twitter Typeahead.js v0.10
date: '2014-02-25T10:26:00-08:00'
tags:
  - ajax
  - twitter
  - twitter typeahead
  - twitter typeahead examples
  - twitter typeahead.js
  - twitter typeahead.js new version
  - twitter typeahead.js v0.10
  - twitter typeahed.js example
  - twitter typehead example
  - twitter typehead.js 0.10
  - twitter typehead.js examples
  - typeahead
  - typeahead.js v0.10
  - typehead.js new version
---
<a title="Twitter Typeahead.js" href="http://twitter.github.io/typeahead.js/" target="_blank">Twitter Typeahead.j</a>s has been updated and there are a lot of changes.  I'm going to give a an explanation for a few of their examples and explain the key features and changes that they have made.  For those looking to upgrade to this new version it will require a rewrite of your current Typeahead.js methods so please be aware of that.  The biggest changes are outlined in their <a title="Typeahead.js Changelog" href="https://github.com/twitter/typeahead.js/blob/master/CHANGELOG.md" target="_blank">changelog</a>.
<blockquote>The most important change in 0.10.0 is that typeahead.js was broken up into 2 individual components: Bloodhound and jQuery#typeahead. Bloodhound is an feature-rich suggestion engine. jQuery#typeahead is a jQuery plugin that turns input controls into typeaheads.</blockquote>
Let's head to the <a title="Typeahead.js Examples" href="http://twitter.github.io/typeahead.js/examples/" target="_blank">examples</a>.
<pre class="lang:default decode:true">// instantiate the bloodhound suggestion engine
var numbers = new Bloodhound({
  datumTokenizer: function(d) { return Bloodhound.tokenizers.whitespace(d.num); },
  queryTokenizer: Bloodhound.tokenizers.whitespace,
  local: [
    { num: 'one' },
    { num: 'two' },
    { num: 'three' },
    { num: 'four' },
    { num: 'five' },
    { num: 'six' },
    { num: 'seven' },
    { num: 'eight' },
    { num: 'nine' },
    { num: 'ten' }
  ]
});

// initialize the bloodhound suggestion engine
numbers.initialize();

// instantiate the typeahead UI
$('.example-numbers .typeahead').typeahead(null, {
  displayKey: 'num',
  source: numbers.ttAdapter()
});</pre>
The most basic of basic.  Here we have a static list of local data.  Bloodhound needs to be initialized with your data and you can see it handles splitting data up by white space with their tokenizers.  This makes things a lot easier because in previous versions you needed to make sure the tokens to be searched through were already split up by white space.  The Bloodhound object is then initialized and the typeahead object is created.  This is a lot more code but it makes the search more robust as we will see with a more advanced example using prefetch and remote data.
<pre class="lang:default decode:true">var films = new Bloodhound({
  datumTokenizer: function(d) { return Bloodhound.tokenizers.whitespace(d.value); },
  queryTokenizer: Bloodhound.tokenizers.whitespace,
  remote: '../data/films/queries/%QUERY.json',
  prefetch: '../data/films/post_1960.json'
});

films.initialize();

$('.example-films .typeahead').typeahead(null, {
  displayKey: 'value',
  source: films.ttAdapter(),
  templates: {
    suggestion: Handlebars.compile(
      '&lt;p&gt;&lt;strong&gt;{{value}}&lt;/strong&gt; – {{year}}&lt;/p&gt;'
    )
  }
});</pre>
Here we see a very similar example to the one above except that we are getting our data from a URL.  The prefetch variable works to get a set of data and cache it locally.  Even if the page is reloaded that data stays with the user's browser until it expires or is removed.  As the user begins their search it will have that data ready and on hand but if there is no data to be found in the prefetch Typeahead.js has another URL, stored in the remote variable, that it can make a query to and get data back about what the user is searching for.  These two in conjunction work fantastically well for supplying the user with a set of data that has the speed of being static but can be dynamic by querying the URL for more data as the user goes.  Again we initialize the Bloodhound object and add it to the typeahead initialization.  We are again using the Handlebars template rendering engine to make the search results look great so that hasn't changed.

The changes are drastic but not hard.  This is a major update to Twitter Typeahead.js and because of that there is deprecation.  My <a title="Twitter Typeahead.js" href="http://ericsaupe.com/twitter-typeahead-js/" target="_blank">other</a> <a title="Using Twitter Typeahead.js Custom Event Triggers" href="http://ericsaupe.com/using-twitter-typeahead-js-custom-event-triggers/" target="_blank">posts</a> are still useful for older versions but may not apply to versions going forward.  I hope this was helpful in getting you up to speed with the latest version of Twitter Typeahead.js.  If you have any questions or comments feel free to leave them below.
