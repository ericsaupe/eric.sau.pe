---
templateKey: blog-post
title: Rails Multipart Layouts and Yielding Sections
date: '2014-08-14T10:35:00-07:00'
tags:
  - rails
  - rails 4
  - rails layouts
  - rails multi section layout
  - rails partial
  - rails render
  - rails render example
  - rails render section
  - rails yield section
  - ror 4
  - ruby on rails
---
One of the great strengths of Rails is the ability to create layouts that house various views of content. You create a layout with a basic HTML structure and then simply drop in your new content based on the view you are rendering. One of the simplest layouts is one that just renders the view that takes up the whole page and this is accomplished by the auto generated
<code>app/views/layouts/application.html.erb</code>.
<pre class="lang:default mark:11 decode:true">&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;title&gt;Myapp&lt;/title&gt;
  &lt;%= stylesheet_link_tag    "application", media: "all", "data-turbolinks-track" =&gt; true %&gt;
  &lt;%= javascript_include_tag "application", "data-turbolinks-track" =&gt; true %&gt;
  &lt;%= csrf_meta_tags %&gt;
&lt;/head&gt;
&lt;body&gt;

&lt;%= yield %&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
The highlighted line is the important one. That &lt;%= yield %&gt; tells Ruby on Rails that whatever view you load just drop it right there. That's great! Now we have a simple base layout that houses our imports and basic structure and CSS and we simply drop in our own content inside of this.

Let's say we want to take it a step further. What if we want a left and a right side bar which is unique to each of our views? We just need to change our basic layout a little bit to add those columns and then update our view template code to take advantage of our new structure.
<h3>Yield Sections</h3>
<code>app/views/layouts/application.html.erb</code>
<pre class="lang:default mark:11-21 decode:true">
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;title&gt;Myapp&lt;/title&gt;
  &lt;%= stylesheet_link_tag    "application", media: "all", "data-turbolinks-track" =&gt; true %&gt;
  &lt;%= javascript_include_tag "application", "data-turbolinks-track" =&gt; true %&gt;
  &lt;%= csrf_meta_tags %&gt;
&lt;/head&gt;
&lt;body&gt;

&lt;div id="left" class="sidebar"&gt;
  &lt;%= yield :left %&gt;
&lt;/div&gt;

&lt;div id="content"&gt;
  &lt;%= yield %&gt;
&lt;/div&gt;

&lt;div id="right" class="sidebar"&gt;
  &lt;%= yield :right %&gt;
&lt;/div&gt;

&lt;/body&gt;
&lt;/html&gt;
</pre>
<code>app/views/MODEL/new.html.erb</code>
<pre class="lang:default decode:true">
&lt;% content_for :left do %&gt;
  &lt;h2&gt;Left Side&lt;/h2&gt;
  &lt;ul&gt;
    &lt;li&gt;...&lt;/li&gt;
  &lt;/ul&gt;
&lt;% end %&gt;

&lt;% content_for :right do %&gt;
  &lt;h2&gt;Right Side&lt;/h2&gt;
  &lt;ul&gt;
    &lt;li&gt;...&lt;/li&gt;
  &lt;/ul&gt;
&lt;% end %&gt;

&lt;h1&gt;Content Heading&gt;&lt;/h1&gt;
</pre>
Following the suggestion of Obie Fernandez and Kevin Faustino in their book <a href="http://www.amazon.com/gp/product/0321944275/ref=as_li_tl?ie=UTF8&amp;camp=1789&amp;creative=390957&amp;creativeASIN=0321944275&amp;linkCode=as2&amp;tag=adveoftheleat-20&amp;linkId=X7PNEYMUPC6IA74Y">The Rails 4 Way</a><img style="border: none !important; margin: 0px !important;" src="http://ir-na.amazon-adsystem.com/e/ir?t=adveoftheleat-20&amp;l=as2&amp;o=1&amp;a=0321944275" alt="" width="1" height="1" border="0" />, I have placed the two named structures of the layout first. It makes it easy to distinguish what the smaller sections will contain before getting to the meat of the content. Everything that isn't labeled as <code>content_for</code> will be dropped in that unnamed <code>yield</code> in the layout.
<h3>Render Partials</h3>
That's all well and good but what if you don't want a specific set of sidebar content for every single view. Maybe a shared partial for the right bar will work great. In that case you'll need to use the render method in your template to generate the template for you. Here is the example from above but using a shared right side bar template.

<code>app/views/layouts/application.html.erb</code>
<pre class="lang:default mark:20 decode:true">&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;title&gt;Myapp&lt;/title&gt;
  &lt;%= stylesheet_link_tag    "application", media: "all", "data-turbolinks-track" =&gt; true %&gt;
  &lt;%= javascript_include_tag "application", "data-turbolinks-track" =&gt; true %&gt;
  &lt;%= csrf_meta_tags %&gt;
&lt;/head&gt;
&lt;body&gt;

&lt;div id="left" class="sidebar"&gt;
  &lt;%= yield :left %&gt;
&lt;/div&gt;

&lt;div id="content"&gt;
  &lt;%= yield %&gt;
&lt;/div&gt;

&lt;div id="right" class="sidebar"&gt;
  &lt;%= render 'shared/right' %&gt;
&lt;/div&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
<code>app/views/shared/_right.html.erb</code>
<pre class="lang:default decode:true">&lt;h2&gt;Right Side&lt;/h2&gt;
&lt;ul&gt;
  &lt;li&gt;...&lt;/li&gt;
&lt;/ul&gt;</pre>
<h3>Render Partials with Variables</h3>
You can even pass in variables to your partials. Let's say our right side bar takes advantage of some user data. We just pass the user variable to the render with a comma and a hash type variable being passed.

<code>app/views/layouts/application.html.erb</code>
<pre class="lang:default mark:20 decode:true">&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;title&gt;Myapp&lt;/title&gt;
  &lt;%= stylesheet_link_tag    "application", media: "all", "data-turbolinks-track" =&gt; true %&gt;
  &lt;%= javascript_include_tag "application", "data-turbolinks-track" =&gt; true %&gt;
  &lt;%= csrf_meta_tags %&gt;
&lt;/head&gt;
&lt;body&gt;

&lt;div id="left" class="sidebar"&gt;
  &lt;%= yield :left %&gt;
&lt;/div&gt;

&lt;div id="content"&gt;
  &lt;%= yield %&gt;
&lt;/div&gt;

&lt;div id="right" class="sidebar"&gt;
  &lt;%= render 'shared/right', user: user %&gt;
&lt;/div&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
<code>app/views/shared/_right.html.erb</code>
<pre class="lang:default decode:true">&lt;h2&gt;Right Side&lt;/h2&gt;
&lt;h3&gt;&lt;%= user.name %&gt;&lt;/h3&gt;
&lt;ul&gt;
  &lt;li&gt;...&lt;/li&gt;
&lt;/ul&gt;</pre>
Pretty cool, right? I hope this introduction to layouts and partials was helpful. If there are any problems with my examples or if you have any questions please feel free to leave a comment below.
