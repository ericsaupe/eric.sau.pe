---
templateKey: blog-post
title: Django 1.6 Update Notes
date: '2013-11-15T15:09:00-08:00'
tags:
  - django
  - django 1.5 to 1.6
  - django 1.6
  - django 1.6 change notes
  - django 1.6 release
  - django 1.6 update
  - django 1.6 upgrade
---
<a title="Django 1.6 Released" href="https://www.djangoproject.com/weblog/2013/nov/06/django-16-released/" target="_blank">Django finally released version 1.6</a> of their Python based web-framework.  I use it at my job everyday and we ran into issues with the old system where they were using such an old version of Django that it was impossible to just simply update.  One of the goals with the new system we are creating is to stay as up to date as we possibly can with all of our technologies.  For us that is Bootstrap, jQuery, Django, and a few other third-party web plugins.  Here are a few of the notes I'm writing down for myself and for others to use if they run into issues in upgrading from 1.5.4 to 1.6.

<h3><a href="https://docs.djangoproject.com/en/1.6/releases/1.6/#default-session-serialization-switched-to-json" target="_blank">Default session serialization switched to JSON</a></h3>

Django changed the way the session variable is being stored from pickle to JSON.  This is for the best as there is an exploit if the attacker knows your SECRET_KEY and has access to the CSRF tokens being sent by users and the session variable uses pickle.  Using JSON fixes this and Django has elected to make it the new default.  The issue is that some things, in our case timezones, are not JSON serializable.  If you'd like to still use pickle for your session variables just add <code>SESSION_SERIALIZER = 'django.contrib.sessions.serializers.PickleSerializer'</code> to your <code>settings.py</code> file.

<h3>Simplejson Deprecated</h3>

Simplejson has been deprecated and you should now use JSON.  This means you'll need to change your imports from <code>from django.utils import simplejson</code> to just <code>import json</code> and you'll need to change any code that uses simplejson to just json.

<h3>User Profile Deprecated</h3>

They have officially deprecated the use of user profiles attached to the default user in favor of custom user models.  Custom user models should be setup on initial project creation.  If you didn't do this then you can use something like South to migrate your data.  The easiest way to create a custom user model is shown below by extending the base Django user and just adding to it.

<h4>settings.py</h4>

<pre><code>AUTH_USER_MODEL = 'myapp.CustomUser'</code></pre>

<h4>models.py</h4>

<pre><code>from django.contrib.auth.models import AbstractUser



class CustomUser(AbstractUser):

   keyboard_shortcuts = models.BooleanField(default=True)</code></pre>

Now anywhere you would make a foreign key to User you'll need to import settings and use settings.AUTH_USER_MODEL instead of User.

<h4>models.py</h4>

<pre><code>from django.conf import settings



class SomeData(models.Model):

\    ....

\    user = models.ForeignKey(settings.AUTH_USER_MODEL)</code></pre>

Those are all of the major things that I needed to change to update everything.  Granted they started to deprecate user profiles back in 1.5 we just hadn't made the change yet and needed to now.  Hope this helps and if you find any other bugs or awkwardness when upgrading post in the comments below.  You can also read the <a title="Django 1.6 Release Notes" href="https://docs.djangoproject.com/en/dev/releases/1.6/" target="_blank">Django 1.6 release notes</a> on their site.
