---
templateKey: blog-post
title: Django Translation Creating Locale Folders
date: '2013-06-27T11:45:00-07:00'
tags:
  - django
  - i18n
  - translation
---
As our huge project progresses we need to start thinking about some of the major features that our users would like to see.  The first is less of a feature and more of a necessity.  Our users are located all over the world and speak four different languages.  To accommodate all of these users we need to translate every bit of text on every page of our application for all of our users.  Luckily for us Django handles translation very nicely.  Read up on the <a title="Django Translation" href="https://docs.djangoproject.com/en/dev/topics/i18n/translation/">Django translation here</a>.

This post will only cover the template tag <code>{% trans %}</code>, more will come as we develop more of our translation setup.

Every template generated will need the tag <code>{% load i18n %}</code> at the top to have access to the <code>trans</code> tag.  From this point on any text string will need to be surrounded by the <code>trans</code> tag.  For example, my button has the code `<button type="button">Home</button>` and to use the translation I simply change it to `<button type="button">{% trans "Home" %}</button>`.  That string after the <code>trans</code> is going to be the id for the soon-to-be-created translation files.  This string can contain spaces and for our purposes we are going to make the id the default English string.  When a translation is not found it will display the id so in our case the correct English string will save time.

Once you've done all of that, and every string you want translated is contained in a <code>trans</code> tag, we need to generate a translation list for all of those strings.  Using Django 1.5.1 get into your application directory inside of your Django project.  Use the command <code>django-admin.py makemessages -l < language code ></code>.  This will generate a folder named the language code inside of a locale folder inside of your application.

OK, now for the reason I wanted to write this post.  I had a list of languages and was trying to test all of this out.  Things were working great for English and German using the language codes en-us and de but was failing for Chinese Simplified, zh-cn.  The reason is actually because though the language value is zh-cn do <em>not</em> use that language code when creating your message folder!  It actually wants to look into the folder zh_CN.  Language folders must be created with the language followed by and underscore and the version.  Since I am not using any translation files for English it doesn't matter what I use but if I were to do English for the United States and English for the UK I would use the makemessages command and create the folders en_US and en_UK.

Just a quick note I wanted to make so that others, and myself, won't fall into this misstep in the future.
