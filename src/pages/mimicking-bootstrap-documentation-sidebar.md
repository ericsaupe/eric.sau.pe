---
templateKey: blog-post
title: Mimicking Bootstrap Documentation Sidebar
date: '2014-02-19T10:24:00-08:00'
tags:
  - bootstrap
  - bootstrap 3
  - bootstrap 3 docs
  - bootstrap 3 docs nav
  - bootstrap 3 docs side nav
  - bootstrap 3 side nav
  - bootstrap 3 sidenav
  - bootstrap affix
  - bootstrap css
  - bootstrap docs
  - bootstrap docs nav
  - bootstrap docs navigation
  - bootstrap docs sidebar
  - bootstrap scrollspy
  - bootstrap side nav
  - bootstrap sidenav
---
If you've spent any time looking at the <a title="Bootstrap" href="http://getbootstrap.com/getting-started/" target="_blank">Bootstrap docs</a> you'll notice their nav bar on the right.  Very slick example of both <a title="Bootstrap Scrollspy" href="http://getbootstrap.com/javascript/#scrollspy" target="_blank">Scrollspy</a> and <a title="Bootstrap Affix" href="http://getbootstrap.com/javascript/#affix" target="_blank">Affix</a>.  I have written blog posts about both of these topics in conjunction with each other titled <a title="Bootstrap ScrollSpy Pitfalls and Fixes" href="http://ericsaupe.com/bootstrap-scrollspy-pitfalls-and-fixes/" target="_blank">Bootstrap ScrollSpy Pitfalls and Fixes</a>.  In my example I use a very simple styling for my nav using nav-pills.  This works out alright but what if we want to get exactly what Bootstrap is doing?  Well, let's figure out just how they do it.

They are using very similar code to what I had in the pitfalls and tricks post so I'll bring that in here.
<pre class="lang:default decode:true">&lt;div id="side-nav" class="col-sm-2 hidden-xs hidden-sm hidden-print affix"&gt;
    &lt;ul class="nav"&gt;
        &lt;li&gt;&lt;a href="#info"&gt;Info&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#properties"&gt;Company Properties&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#genome"&gt;Buyers/Suppliers&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#blog"&gt;Recent Blog&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#tests"&gt;Total Tests&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#company-bio"&gt;Company Bio&lt;/a&gt;&lt;/li&gt;
   &lt;/ul&gt;
&lt;/div&gt;

&lt;div id="company-content" class="col-xs-12 col-sm-12 col-md-10"&gt;
    &lt;a name="info" id="info"&gt;&lt;/a&gt;
    &lt;h4&gt;Info&lt;/h4&gt;
    ...
    &lt;a name="properties" id="properties"&gt;&lt;/a&gt;
    &lt;h4&gt;Company Properties&lt;/h4&gt;
    ...

    etc.
&lt;/div&gt;</pre>
Now that we have a simple navigation let's style it to have sub menus not be shown until the item is in view of the user, just like Bootstrap.
<pre class="lang:default decode:true">&lt;div id="side-nav" class="col-sm-2 hidden-xs hidden-sm hidden-print affix"&gt;
    &lt;ul class="nav"&gt;
        &lt;li&gt;&lt;a href="#info"&gt;Info&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;
            &lt;a href="#properties"&gt;Company Properties&lt;/a&gt;
            &lt;ul class="nav"&gt;
                &lt;li&gt;&lt;a href="#properties-kudos"&gt;Kudos&lt;/a&gt;&lt;/li&gt;
                &lt;li&gt;&lt;a href="#properties-awards"&gt;Awards&lt;/a&gt;&lt;/li&gt;
                &lt;li&gt;&lt;a href="#properties-buildings"&gt;Buildings&lt;/a&gt;&lt;/li&gt;
            &lt;/ul&gt;
        &lt;/li&gt;
        &lt;li&gt;&lt;a href="#genome"&gt;Buyers/Suppliers&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#blog"&gt;Recent Blog&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#tests"&gt;Total Tests&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#company-bio"&gt;Company Bio&lt;/a&gt;&lt;/li&gt;
   &lt;/ul&gt;
&lt;/div&gt;

&lt;div id="company-content" class="col-xs-12 col-sm-12 col-md-10"&gt;
    &lt;a name="info" id="info"&gt;&lt;/a&gt;
    &lt;h4&gt;Info&lt;/h4&gt;
    ...
    &lt;a name="properties" id="properties"&gt;&lt;/a&gt;
    &lt;h4&gt;Company Properties&lt;/h4&gt;
    ...

    etc.
&lt;/div&gt;</pre>
We've added some sub menu items to a few of the nav elements.  At this point if you were to render the page it would show all of the items in the list regardless of it being a sub menu or not.  We want to change that so we only see specific sub menus when they are active.  To do this we just need to add a little bit of CSS that will trigger the display of the sub menus.  When the menu item is not active the sub menus are hidden.  When the menu item is activated it displays the sub menus below.
<pre class="lang:default decode:true">&lt;style&gt;
    #side-nav .nav&gt;.active&gt;a, #side-nav .nav&gt;.active:hover&gt;a, #side-nav .nav&gt;.active:focus&gt;a {
        padding-left: 18px;
        font-weight: 700;
        color: #563d7c;
        background-color: transparent;
        border-left: 2px solid #563d7c;
    }
    #side-nav .nav .nav&gt;.active&gt;a, #side-nav .nav .nav&gt;.active:hover&gt;a, #side-nav .nav .nav&gt;.active:focus&gt;a {
        font-weight: 500;
        padding-left: 28px;
    }

    #side-nav .nav&gt;li&gt;a {
        display: block;
        font-size: 13px;
        font-weight: 400;
        color: #999;
        padding: 4px 20px;
    }
    #side-nav .nav .nav {
        display: none;
        padding-bottom: 10px;
    }
    #side-nav .nav&gt;.active&gt;.nav {
        display: block;
        padding-bottom: 10px;
    }
&lt;/style&gt;

&lt;div id="side-nav" class="col-sm-2 hidden-xs hidden-sm hidden-print affix"&gt;
    &lt;ul class="nav"&gt;
        &lt;li&gt;&lt;a href="#info"&gt;Info&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;
            &lt;a href="#properties"&gt;Company Properties&lt;/a&gt;
            &lt;ul class="nav"&gt;
                &lt;li&gt;&lt;a href="#properties-kudos"&gt;Kudos&lt;/a&gt;&lt;/li&gt;
                &lt;li&gt;&lt;a href="#properties-awards"&gt;Awards&lt;/a&gt;&lt;/li&gt;
                &lt;li&gt;&lt;a href="#properties-buildings"&gt;Buildings&lt;/a&gt;&lt;/li&gt;
            &lt;/ul&gt;
        &lt;/li&gt;
        &lt;li&gt;&lt;a href="#genome"&gt;Buyers/Suppliers&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#blog"&gt;Recent Blog&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#tests"&gt;Total Tests&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="#company-bio"&gt;Company Bio&lt;/a&gt;&lt;/li&gt;
   &lt;/ul&gt;
&lt;/div&gt;

&lt;div id="company-content" class="col-xs-12 col-sm-12 col-md-10"&gt;
    &lt;a name="info" id="info"&gt;&lt;/a&gt;
    &lt;h4&gt;Info&lt;/h4&gt;
    ...
    &lt;a name="properties" id="properties"&gt;&lt;/a&gt;
    &lt;h4&gt;Company Properties&lt;/h4&gt;
    ...

    etc.
&lt;/div&gt;</pre>
That's all there is to it! No tricky JavaScript required, it's all CSS.   Now just adjust the padding, change the color, and add sub menus to your heart's content.  You now have a side nav that looks and acts just like Bootstrap's does in their docs.
