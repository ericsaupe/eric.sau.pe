---
templateKey: blog-post
title: Create a Child Theme in Wordpress
date: '2013-11-07T15:07:00-08:00'
tags:
  - wordpress
  - wordpress child theme
  - wordpress theme
---
<a href="http://wordpress.org/" target="_blank">Wordpress</a> is a great way to get a site up and running quickly.  A lot of people think it is just for blogs but it can be whatever you'd like it to be with its customizable themes and content manager.  There are hundreds of Wordpress themes online and a lot of times you might really like everything about a theme except for a few changes.  To make those changes the best thing to do is create a <a href="http://codex.wordpress.org/Child_Themes" target="_blank">child theme</a>.

A child theme is a way to update and make changes to a base theme.  There are just a few steps you need to take to create a child theme and then you'll be ready to make changes.
<ul>
	<li>Create a directory in your themes folder for your child theme
<ul>
	<li>The general naming convention is to name the folder the name of the theme with a -child appended to the end. Example: greattheme-child.</li>
</ul>
</li>
	<li>In the child directory create a <code>style.css</code> file.  It must start with the following lines.</li>
</ul>
<pre class="lang:default decode:true ">/*
Theme Name: Great Theme Child
Theme URI: http://example.com/great-theme-child/
Description: Great Theme Child Theme
Author: Eric Saupe
Author URI: http://example.com
Template: greattheme
Version: 1.0.0
*/

@import url("../greattheme/style.css");

/* =Theme customization starts here
-------------------------------------------------------------- */</pre>
Be sure to change each of the lines to suit your theme's needs.
<ul>
	<li>This stylesheet includes everything from the parent stylesheet so this is just to override or add to the parent theme.</li>
</ul>
Your child theme can override any of the files found in the parent theme.  Most of the time this includes the <code>functions.php</code> file.  For <code>functions.php</code> it does not override it but instead is loaded <em>in addition to and after</em> the parent <code>functions.php</code>.  The <code>functions.php</code> file is a basic PHP file with an opening and closing PHP tag with all of your PHP in the middle.

Creating the folder for your child theme with the <code>style.css</code> will let it show up on your Wordpress theme selection.  When the parent gets updated you shouldn't need to update anything for your child because they are loaded together.  You can make all the changes you want to the base theme.  Hope this helps people get started on changing and customizing a theme for Wordpress.  If you have any thoughts or comments on creating child themes in Wordpress leave them below.
