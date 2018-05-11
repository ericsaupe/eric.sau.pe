---
templateKey: blog-post
title: Twitter Typeahead.js
date: '2013-08-08T11:00:00-07:00'
tags:
  - ajax
  - bootstrap
  - bootstrap 3
  - twitter
  - twitter typeahead
  - typeahead
  - typeahead.js
---
![](/img/typeaheadjs-300x102.png)

<h2><a title="Twitter Typeahead.js v0.10" href="http://ericsaupe.com/twitter-typeahead-js-v0-10/">New Version - Twitter Typehead.js v0.10</a></h2>
Bootstrap dropped support for their bootstrap-typeahead.js and suggested we all use <a title="Twitter Typeahead.js" href="http://twitter.github.io/typeahead.js/" target="_blank">Twitter's Typeahead.js</a>.  This was a pretty big change for us and we encountered a bit of a learning curve because the documentation wasn't exactly what we had hoped it would be.  Typeahead.js is incredible and can do some amazing things though and we are happy to be using it.

The first issue is that right out of the box it will not work with Bootstrap 3.  Support is still being looked at but in the mean time there are some fixes.  Here is <a title="Bootstrap 3 Compatability" href="https://github.com/twitter/typeahead.js/issues/378" target="_blank">a thread about the issue on their GitHub page</a> with some helpful people posting solutions for the interim.  Below is the code we are using for ours which I'm sure will change in the coming days.

<pre class="lang:css decode:true">//Twitter Typeahead CSS
//Updated 22 August 2013
.twitter-typeahead {
    width: 100%;
    position: relative;
}

.twitter-typeahead .tt-query,
.twitter-typeahead .tt-hint {
    margin-bottom: 0;
    width:100%;
    height: 34px;
    position: absolute;
    top:0;
    left:0;
}

.twitter-typeahead .tt-hint {
    color:#a1a1a1;
    z-index: 1;
    padding: 6px 12px;
    border:1px solid transparent;
}

.twitter-typeahead .tt-query {
    z-index: 2;
    border-radius: 4px!important;
    /* add these 2 statements if you have an appended input group */
    border-top-right-radius: 0!important;
    border-bottom-right-radius: 0!important;
    /* add these 2 statements if you have an prepended input group */
    /* border-top-left-radius: 0!important;
    border-bottom-left-radius: 0!important; */
}

.tt-dropdown-menu {
    min-width: 160px;
    margin-top: 2px;
    padding: 5px 0;
    background-color: #fff;
    border: 1px solid #ccc;
    border: 1px solid rgba(0,0,0,.2);
    *border-right-width: 2px;
    *border-bottom-width: 2px;
    -webkit-border-radius: 6px;
    -moz-border-radius: 6px;
    border-radius: 6px;
    -webkit-box-shadow: 0 5px 10px rgba(0,0,0,.2);
    -moz-box-shadow: 0 5px 10px rgba(0,0,0,.2);
    box-shadow: 0 5px 10px rgba(0,0,0,.2);
    -webkit-background-clip: padding-box;
    -moz-background-clip: padding;
    background-clip: padding-box;
}

.tt-suggestion {
    display: block;
    padding: 3px 20px;
}

.tt-suggestion.tt-is-under-cursor {
    color: #fff;
    background-color: #0081c2;
    background-image: -moz-linear-gradient(top, #0088cc, #0077b3);
    background-image: -webkit-gradient(linear, 0 0, 0 100%, from(#0088cc), to(#0077b3));
    background-image: -webkit-linear-gradient(top, #0088cc, #0077b3);
    background-image: -o-linear-gradient(top, #0088cc, #0077b3);
    background-image: linear-gradient(to bottom, #0088cc, #0077b3);
    background-repeat: repeat-x;
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff0088cc', endColorstr='#ff0077b3', GradientType=0)
}

.tt-suggestion.tt-is-under-cursor a {
    color: #fff;
}

.tt-suggestion p {
    margin: 0;
}</pre>

Once we got the formatting and could create our typeahead inputs we needed data for them.  We were using AJAX calls to our server to get the list as the user typed.  This worked but was a little slow.  Typeahead now has a few different ways of populating data into the input field.

<h2>Local</h2>
Local is a list of hard coded values right in your JavaScript
<h2>Prefetch</h2>
When the page is loaded the ajax call is made and stored in the browser's local storage so the next time you visit the page it does not need to make another request unless the time-to-live has expired.  The ttl is defaulted to one day but you can change that in the options you pass.  One thing to note is that your browser has a limited amount of storage for this type of thing.  When we tried to pull our contacts list all in one go it would crash typeahead because the browser ran out of memory for local storage.  Our fix for this was that the prefetch URL would only return a limited number of entries, for example, the last 1000 updated contacts.
<h2>Remote</h2>
If the user starts to try and type something that isn't immediately available through prefetch or local it falls back to your remote URL.  Just place the %QUERY tag in your URL and it will take what the user has entered and make a call to your server for a list using that query.
<h2>Full Example:</h2>
<pre class="lang:js decode:true">$("#customer_search").typeahead({
    name: "customers", //Local storage name so that other parts of your site don't need to make a request if we already have it
    remote: "customerlist/%QUERY/", //Fall back in case prefetch doesn't have the necessary results
    prefetch: "customerlist/", //Limited to last 1000 updated contacts
    limit: 10, //This is the limit displayed not returned
    template: "&lt;p&gt;&lt;strong&gt;{{number}}&lt;/strong&gt; - {{name}}", //Typeahead inline template
    engine: Hogan,
}).on("typeahead:selected", function($e, datum){ //What to do on select
    window.location = "overview/" + datum['number'] + "/";
});</pre>
<h2>Templates</h2>
Typeahead supports your own custom templates for the drop down.  In our case we are just making the customer number bold and displaying their name but you could do everything from changing fonts, making inline lists, or even displaying images inline with your results.  Very cool.
<h2>Datums</h2>
We learned the hard way about a few different things when formatting results for your typeahead.  Typeahead expects a list of dictionaries with certain values, these are called datums.  Only two keys are required by typeahead, value and tokens.  After that you can add your own keys to be used by the typeahead for whatever purpose you'd like, whether it's changing what happens on click or an image to be placed inline.  The value is the actual value of the typeahead, what the user is trying to get to.  Tokens are a list of strings that help the user get to where they want to go.  One very important note, the tokens must be single word strings -- no spaces.   Spaces come across as %20 and when the user enters a normal space the typeahead doesn't recognize that correctly.  So what we did was split the name of the company/contact into a list of separate strings for each word and that worked great.
<pre class="lang:python decode:true">for customer in customers:
    output.append({'value':'%s' % customer.name,
                   'tokens':customer.name.split() + ['%s' % customer.customer_number,],
                   'name':customer.name,
                   'number':customer.customer_number})</pre>
<h2>Onward</h2>
Still playing around with it but I feel like we have a pretty good grasp on it.  Here is a list of <a title="Typeahead.js Examples" href="http://twitter.github.io/typeahead.js/examples/" target="_blank">their examples</a> just to help drive the point home a bit further. Just waiting on the Bootstrap 3 support but overall we are happy with the new typeahead after we spent a long time trying to figure out all of the quirks we were having with it.

Hope this is helpful to others out there.

<strong>EDIT 16 September 2013</strong>

Some people were complaining about the dropdown not dynamically scaling.  Git user <a href="https://github.com/karimlahlou" target="_blank">karimlahlou</a> has posted this to fix that problem.

<blockquote>Thank you <a href="https://github.com/ashleydw">@ashleydw</a>, finally i got it working. for those need to adjust the list width, use this function:
<pre class="lang:js decode:true ">$('.typeahead').typeahead({
    remote: 'path/to/url'
}).on('typeahead:opened',function(){$('.tt-dropdown-menu').css('width',$(this).width() + 'px');});</pre>
<span style="line-height: 1.5em;">Hope this helps a few people.</span></blockquote>
