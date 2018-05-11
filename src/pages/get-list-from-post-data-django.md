---
templateKey: blog-post
title: Get List from POST Data - Django
date: '2013-09-11T11:36:00-07:00'
tags:
  - django
  - django form list
  - django getlist
  - django post list
---
Recently I tried sending a list through a form using something like <code>&lt;input name="my_list[]" value="some val"&gt;</code>.  When this form is submitted all of the inputs with that name are grouped together for easy access.  In Django, one would think you could simply access the list using <code>request.POST['my_list[]']</code>.  <em>This is not the case. </em>Using that command will give you the last value.  <a href="https://code.djangoproject.com/ticket/1130" target="_blank">Django says this is a feature</a> and to get the list use <code>request.POST.getlist('my_list[]')</code>.

Just an FYI and reminder to myself.
