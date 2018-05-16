---
templateKey: blog-post
title: Adding Custom Django 404 and 500 Pages
date: '2013-12-18T15:16:00-08:00'
tags:
  - django
  - django 404
  - django 500
  - django custom error pages
---
Django comes with a default 404 and 500 error pages which are dull and boring.  It would be much better if you add your own spin on those pages to inform your user of what happened.  Django makes this super easy by searching your project template files for a 404.html or 500.html file when it tries to render those pages.  This means all you need to do is make a nice Django template called 404.html and 500.html and place it in your project templates folder and you're done!  You'll have access to whatever context processors you are running for information, most of the time these pages won't be user specific and will just display some error.

Please note that you won't be able to see them unless you turn <code>DEBUG = False</code> in your <code>settings.py</code> file.  Once debug is off Django will render the 404 and 500 pages rather than the debug output.

Hope this helps people get their pages a little more user friendly and comfortable.
