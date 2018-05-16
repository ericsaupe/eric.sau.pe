---
templateKey: blog-post
title: Bootstrap Tabs with Dynamic Content Loading
date: '2013-09-17T14:48:00-07:00'
tags:
  - bootstrap
  - bootstrap 3
  - bootstrap 3 ajax
  - bootstrap 3 tabs
  - bootstrap 3 tabs example
  - bootstrap 3 tabs example ajax
  - bootstrap ajax tabs
  - bootstrap javascript tabs
  - bootstrap tabs
  - bootstrap tabs ajax example
  - javascript
  - jquery
---
Bootstrap comes with a lot of different JavaScript tools to make your website more user friendly.  Today I'm going to cover <a title="Bootstrap Tabs" href="http://getbootstrap.com/javascript/#tabs" target="_blank">Tabs</a> and then give a more advanced example for making your tabs have dynamic content using AJAX.  We all know what tabs look like and in a website they can be very useful to group similar content without having to create a new page for each chunk of data.  Below is an example of the basic Bootstrap tabs.
<pre class="lang:default decode:true ">&lt;ul id="myTab" class="nav nav-tabs"&gt;
  &lt;li class="active"&gt;&lt;a href="#home" data-toggle="tab"&gt;Home&lt;/a&gt;&lt;/li&gt;
  &lt;li class=""&gt;&lt;a href="#profile" data-toggle="tab"&gt;Profile&lt;/a&gt;&lt;/li&gt;
  &lt;li class="dropdown"&gt;
    &lt;a href="#" id="myTabDrop1" class="dropdown-toggle" data-toggle="dropdown"&gt;Dropdown &lt;b class="caret"&gt;&lt;/b&gt;&lt;/a&gt;
    &lt;ul class="dropdown-menu" role="menu" aria-labelledby="myTabDrop1"&gt;
      &lt;li&gt;&lt;a href="#dropdown1" tabindex="-1" data-toggle="tab"&gt;@fat&lt;/a&gt;&lt;/li&gt;
      &lt;li&gt;&lt;a href="#dropdown2" tabindex="-1" data-toggle="tab"&gt;@mdo&lt;/a&gt;&lt;/li&gt;
    &lt;/ul&gt;
  &lt;/li&gt;
&lt;/ul&gt;

&lt;div id="myTabContent" class="tab-content"&gt;
  &lt;div class="tab-pane fade active in" id="home"&gt;
    &lt;p&gt;Raw denim you probably haven't heard of them jean shorts Austin. Nesciunt tofu stumptown aliqua, retro synth master cleanse. Mustache cliche tempor, williamsburg carles vegan helvetica. Reprehenderit butcher retro keffiyeh dreamcatcher synth. Cosby sweater eu banh mi, qui irure terry richardson ex squid. Aliquip placeat salvia cillum iphone. Seitan aliquip quis cardigan american apparel, butcher voluptate nisi qui.&lt;/p&gt;
  &lt;/div&gt;
  &lt;div class="tab-pane fade" id="profile"&gt;
    &lt;p&gt;Food truck fixie locavore, accusamus mcsweeney's marfa nulla single-origin coffee squid. Exercitation +1 labore velit, blog sartorial PBR leggings next level wes anderson artisan four loko farm-to-table craft beer twee. Qui photo booth letterpress, commodo enim craft beer mlkshk aliquip jean shorts ullamco ad vinyl cillum PBR. Homo nostrud organic, assumenda labore aesthetic magna delectus mollit. Keytar helvetica VHS salvia yr, vero magna velit sapiente labore stumptown. Vegan fanny pack odio cillum wes anderson 8-bit, sustainable jean shorts beard ut DIY ethical culpa terry richardson biodiesel. Art party scenester stumptown, tumblr butcher vero sint qui sapiente accusamus tattooed echo park.&lt;/p&gt;
  &lt;/div&gt;
  &lt;div class="tab-pane fade" id="dropdown1"&gt;
    &lt;p&gt;Etsy mixtape wayfarers, ethical wes anderson tofu before they sold out mcsweeney's organic lomo retro fanny pack lo-fi farm-to-table readymade. Messenger bag gentrify pitchfork tattooed craft beer, iphone skateboard locavore carles etsy salvia banksy hoodie helvetica. DIY synth PBR banksy irony. Leggings gentrify squid 8-bit cred pitchfork. Williamsburg banh mi whatever gluten-free, carles pitchfork biodiesel fixie etsy retro mlkshk vice blog. Scenester cred you probably haven't heard of them, vinyl craft beer blog stumptown. Pitchfork sustainable tofu synth chambray yr.&lt;/p&gt;
  &lt;/div&gt;
  &lt;div class="tab-pane fade" id="dropdown2"&gt;
    &lt;p&gt;Trust fund seitan letterpress, keytar raw denim keffiyeh etsy art party before they sold out master cleanse gluten-free squid scenester freegan cosby sweater. Fanny pack portland seitan DIY, art party locavore wolf cliche high life echo park Austin. Cred vinyl keffiyeh DIY salvia PBR, banh mi before they sold out farm-to-table VHS viral locavore cosby sweater. Lomo wolf viral, mustache readymade thundercats keffiyeh craft beer marfa ethical. Wolf salvia freegan, sartorial keffiyeh echo park vegan.&lt;/p&gt;
  &lt;/div&gt;
&lt;/div&gt;</pre>
You'll notice that there are two sections to the tabs. The first is the nav-bar ul. This is where you give each tab a title. You can even do drop downs here because they are normal Bootstrap nav items.  That's all there is to the basic Bootstrap tabs.  They work wonders and you can see the <a title="Bootstrap Tabs" href="http://getbootstrap.com/javascript/#tabs" target="_blank">live demo</a> on their site here.

To make each tab load in data via AJAX you'll need to tap into Bootstrap's triggers.  Here's what I did using the same example as above and the JQuery below.
<pre class="lang:js decode:true ">$('a[data-toggle="tab"]').on('shown.bs.tab', function (e) {
  var target = $(e.target).attr("href") // activated tab
  if ($(target).is(':empty')) {
    $.ajax({
      type: "GET",
      url: "/article/",
      error: function(data){
        alert("There was a problem");
      },
      success: function(data){
        $(target).html(data);
      }
  })
 }
})</pre>
The trigger is fired when the tab is changed and the e.target is the event target, or in our case the newly activated tab.  Since the tab's href points to the id of the element we want to change data we can use that to point our data to when it comes.  I've added a check to make sure we are only making the AJAX call if that tab is empty to help on load times of large data sets but you can remove that if you'd like.

That's all there is to it.  Nothing too difficult but useful when you need tabs and need to change that data without reloading the page.  Let me know how you are using tabs below in the comments.
