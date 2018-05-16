---
templateKey: blog-post
title: Custom Column Widths in Bootstrap Tables
date: '2013-09-24T14:50:00-07:00'
tags:
  - bootstrap
  - bootstrap 3
  - bootstrap 3 tables
  - bootstrap table
  - bootstrap tables
  - bootstrap3
  - django
  - django bootstrap
  - html tables
  - tables
---
<iframe width="1867" height="776" src="https://www.youtube.com/embed/uWPeEl9Ok_s" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

One of the biggest things I've run into with Bootstrap are their <a href="http://getbootstrap.com/css/#tables" target="_blank">tables</a> which are dynamic and grow and shrink depending on content size.  For the most part this is completely desired.  Every now and then you want a bit more control over your table column widths.  This is a very simple trick that I wanted to document for those who have run into the problem of wanting more control than Bootstrap gives initially.

Say you've created your table as shown below.
<pre class="lang:default decode:true">&lt;table class="table table-condensed table-striped"&gt;
    &lt;thead&gt;
        &lt;tr&gt;
            &lt;th&gt;Number&lt;/th&gt;
            &lt;th&gt;Standard&lt;/th&gt;
            &lt;th&gt;Category&lt;/th&gt;
            &lt;th&gt;Labs&lt;/th&gt;
            &lt;th&gt;Description&lt;/th&gt;
            &lt;th&gt;Min. Sample Size&lt;/th&gt;
        &lt;/tr&gt;
    &lt;/thead&gt;
    &lt;tbody&gt;
        {% for test in tests %}
            &lt;tr&gt;
                &lt;td&gt;{{ test.number }}&lt;/td&gt;
                &lt;td&gt;{{ test.name }}&lt;/td&gt;
                &lt;td&gt;{{ test.category }}&lt;/td&gt;
                &lt;td&gt;{{ test.labs }}&lt;/td&gt;
                &lt;td&gt;{{ test.description }}&lt;/td&gt;
                &lt;td&gt;{{ test.minimum_sample }}&lt;/td&gt;
            &lt;/tr&gt;
        {% endfor %}
    &lt;/tbody&gt;
&lt;/table&gt;</pre>
Let's say once you render this table the description for the test is way too big and is pushing the other columns away and making them smaller than you think they should be.  The quickest way to fix this is to simply give column sizes to your table headers.  Whatever the width of your table headers your table data will conform to that size.  Knowing this we can alter the above example to make description take up less space and give a little bit more to other columns.
<pre class="lang:default mark:4-9 decode:true">&lt;table class="table table-condensed table-striped"&gt;
    &lt;thead&gt;
        &lt;tr&gt;
            &lt;th class="col-sm-1"&gt;Number&lt;/th&gt;
            &lt;th class="col-sm-2"&gt;Standard&lt;/th&gt;
            &lt;th class="col-sm-2"&gt;Category&lt;/th&gt;
            &lt;th class="col-sm-2"&gt;Labs&lt;/th&gt;
            &lt;th class="col-sm-3"&gt;Description&lt;/th&gt;
            &lt;th class="col-sm-2"&gt;Min. Sample Size&lt;/th&gt;
        &lt;/tr&gt;
    &lt;/thead&gt;
    &lt;tbody&gt;
        {% for test in tests %}
            &lt;tr&gt;
                &lt;td&gt;{{ test.number }}&lt;/td&gt;
                &lt;td&gt;{{ test.name }}&lt;/td&gt;
                &lt;td&gt;{{ test.category }}&lt;/td&gt;
                &lt;td&gt;{{ test.labs }}&lt;/td&gt;
                &lt;td&gt;{{ test.description }}&lt;/td&gt;
                &lt;td&gt;{{ test.minimum_sample }}&lt;/td&gt;
            &lt;/tr&gt;
        {% endfor %}
    &lt;/tbody&gt;
&lt;/table&gt;</pre>
Super simple.  Works great.  Give it a try.  The biggest request I've had, that I have yet to find an answer for, is how to stop the overflow for a table data and show ellipsis instead using Bootstrap.  It's a simple enough procedure in HTML but Bootstrap is doing something else that doesn't allow the overflow.  If anyone knows how to keep a table data on one line and have it show ellipsis please leave a comment.
