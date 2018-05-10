---
templateKey: blog-post
title: Bootstrap 3 Change Notes
date: '2013-07-31T12:15:00-07:00'
tags:
  - bootstrap
  - bootstrap 3
  - modal
  - modals
  - spacing
  - web development
---
![](/img/blurred-code-300x156.png)

After spending the last two days working with Bootstrap 3 I wanted to jot down a few of the things I found in case I forget them tomorrow or to help anyone else running into similar issues.

<h2>Forms, Grid System, and Screen Resolutions</h2>
We have converted our forms from form-horizontal to the default Bootstrap form.  We think it looks better with the new update.

Old code:

<pre><code>&lt;div class="control-group span4"&gt;
    &lt;label class="control-label"&gt;
        {{ form.email.label_tag }}
    &lt;/label&gt;
    &lt;div class="controls"&gt;
        {{ form.email }}
    &lt;/div&gt;
&lt;/div&gt;</code></pre>

New code:

<pre><code>&lt;div class="form-group col-4"&gt;
    {{ form.email.label_tag }}
    {{ form.email }}
&lt;/div&gt;</code></pre>

Cleaned up a bit as we had that extra label tag on our old code which was unnecessary as Django adds the label tag correctly on render.  One of the biggest thing to notice is that the <code>span4</code> tag changed to <code>col-4</code>.  Bootstrap is now responsive by design and mobile first which means that the <code>col-</code> tags are meant for display on a mobile device but are the default when no other tags are found.  This gives us the ability to make pages and forms that appear differently depending on screen resolution.  There are three tags for column spacing, <code>col-</code> for mobile devices, <code>col-sm-</code> for tablet screens, and <code>col-lg-</code> for desktops.  To use the various screen sizes just add on more tags.  The sizes will appear when viewed on different resolutions.  Making the above example take up the whole row on a mobile device and only a third of the screen on a desktop would change the code like so:

<pre><code>&lt;div class="form-group col-12 col-lg-4"&gt;
    {{ form.email.label_tag }}
    {{ form.email }}
&lt;/div&gt;</code></pre>

Easy enough right?  I thought so too!  Not all of us will have a mobile device or tablet ready to go to test our site on different resolutions.  You can always just resize your window to see what happens but that is annoying too.  Luckily, we found a great site that will take any URL and display the page at any screen resolution and has a lot of built-in resolutions for common devices called <a title="Screenfly" href="https://quirktools.com/screenfly/" target="_blank">Screenfly</a>.  This even works for pages hosted locally.

<h2>Modals</h2>
Modals had a lot of changes made to them which made us redo all of our current modals because they were all broken.  Example:

Old code:

<pre><code>&lt;a id="{{ modal.id }}" href="#my_modal" role="button" data-toggle="modal"&gt;Open Modal&lt;/a&gt;

&lt;div id="my_modal" class="modal hide fade" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true"&gt;
    &lt;div class="modal-header"&gt;
        &lt;button type="button" class="close" data-dismiss="modal" aria-hidden="true"&gt;×&lt;/button&gt;
        &lt;h3 id="myModalLabel"&gt;{% trans "My Modal" %}&lt;/h3&gt;
    &lt;/div&gt;
    &lt;div data-id="{{ modal.id }}" class="modal-body my_modal_div"&gt;&lt;/div&gt;
&lt;/div&gt;</code></pre>

New code:

<pre><code>&lt;a id="{{ modal.id }}" href="/myapp/modalbody/{{ modal.id }}/" data-target="#my_modal" data-toggle="modal"&gt;Open Modal&lt;/a&gt;
&lt;div id="my_modal" class="modal fade"&gt;
    &lt;div class="modal-dialog"&gt;
        &lt;div class="modal-content"&gt;
            &lt;div class="modal-header"&gt;
                &lt;button type="button" class="close" data-dismiss="modal" aria-hidden="true"&gt;×&lt;/button&gt;
                &lt;h4 id="modal-label"&gt;{% trans "My Modal" %}&lt;/h4&gt;
            &lt;/div&gt;
            &lt;div data-id="{{ modal.id }}" class="modal-body my_modal_div"&gt;&lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;
&lt;/div&gt;
</code></pre>

A few things to note, the first of which is that the old code was using JavaScript to load in the contents of a page into the modal. Turns our Bootstrap did this already if you just provided the link in the href.  Now we know.  The hide class has been removed.  When the modal is opened Bootstrap adds the modal-open class to the body which stops scrolling on the page and limits it to the modal only.  Pretty awesome.  Make sure your JavaScript files are being loaded at the bottom of your HTML pages to ensure this works correctly.

One thing we have been trying to do is put all Save buttons on the right side of our pages and modals.  Using the Bootstrap pull-right class floats objects to the right.  The problem with this is the modal-content div will not grow with floated objects.  More specifically no outer div will grow with floated elements by default.  To get around this you simply need to add a bit of CSS.

<pre><code>.modal-content {
   overflow: auto;
   padding-bottom: 15px;
}</code></pre>

This works perfectly for any modal with a floated right button at the bottom but if there isn't anything floated at the very bottom of the modal this will add an extra 15px to the padding, which we don't want. To fix that just make that CSS a bit more specific by using id's on the modals you want to specifically fix.  I have made a <a title="jsFiddle Pull-Right Example" href="http://jsfiddle.net/gV6y8/5/" target="_blank">jsFiddle</a> which demonstrates exactly what I am talking about.

<h2>Closing</h2>
That's it so far.  Most of that took a really long time to figure out.  Bootstrap dropped their typeahead JavaScript in favor of <a title="typeahead.js" href="http://twitter.github.io/typeahead.js/" target="_blank">typeahead.js</a> which I'm still trying to figure out fully.  I've got it to work but there are some really weird things happening that I think have to do with Bootstrap 3.  The biggest problem we are running into is that Bootstrap 3 is so new no one has written much about how to fix little problems like the ones we've had so far and other utilities haven't updated to fully support it yet so we soldier on.

Let me know if you have any other tips or if you've had anything that has tripped you up so far.

<h2>EDIT - 8/1/13</h2>
Bootstrap dropped support for their own bootstrap-typeahead.js and has instead suggested typeahead.js by Twitter.  One of the biggest problems with that right now is typeahead.js does not support Bootstrap 3 and so the CSS is messed up.  Here is an <a title="jsFiddle Typeahead.js Problem" href="http://jsfiddle.net/y3mZn/" target="_blank">example</a> I wrote up quickly to demonstrate it and I have commented on their GitHub project page on the <a href="https://github.com/twitter/typeahead.js/issues/378" target="_blank">issue of Bootstrap 3 CSS problems</a>..  They say they are aware of it and the CSS is on their TODO list.  Here's to hoping it happens quickly!
