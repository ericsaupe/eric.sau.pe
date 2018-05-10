---
templateKey: blog-post
title: Django Translation - Going Live!
date: '2013-07-18T12:07:00-07:00'
tags:
  - django
  - i18n
  - translation
---
A while back I wrote a post about Django translation and getting your websites to be translated.  Since then it has been pretty successful in our development version of the site and I have continued to learn the quirks of Django translation.  A couple of things to note:
<ol>
	<li><span style="line-height: 13px;">In your .po files you will occasionally see that some of your strings have a <code>#, fuzzy</code> tag above them.  This means that there is already a translation found elsewhere and it's not sure if it should use your translation file in this app or the other one it knows about.  I had a lot of issues with this so I have been removing the fuzzy tag entirely when I find it and this forces Django to use the translation file as is.</span></li>
	<li>Ordering matters in INSTALLED_APPS.  Django will load translation files in order of your INSTALLED_APPS list.  Django has a set of built in translated strings that it will use first before using your translations.  I found this was causing an issue when I didn't like the translation used for the Chinese version of Email.  Django's version was four characters long and the one I was using would only be two.  I had to rearrange my INSTALLED_APPS to load my translation files first before the Django ones.  Although, now that I think about it, this may be caused by fuzzy tags...I'll look into this.</li>
</ol>
Once we had everything ready some people from the company wanted to see a demo to play around with.  We were going to put everything up and this included translation files just as a demo.  Transferring everything was easy with the new structure of Django since 1.4.  We simply dragged the apps over, updated the settings.py file, and collected static and we were ready to go!  One problem though, when I compiled the messages I was getting an error, and then another error, and another.  Here is a list of the things I did to get translation working on our Linux server.
<ol>
	<li><span style="line-height: 13px;"><code>sudo apt-get install gettext</code> - This is what Django uses to figure out translation and must be installed.</span></li>
	<li>Ensure <code>USE_I18N = True</code> in settings.py.  Ours was but it's good to check.</li>
	<li><code>django.middleware.locale.LocaleMiddleware</code> must be listed in your <code>MIDDLEWARE_CLASSES</code> - This is the kicker that took me a while to figure out.  Since it took me a long time to get this working on my dev server I didn't write down all of the steps, hence this post.  This will not throw errors to your log files either so double-check.</li>
	<li>Update your project <code>urls.py</code> to include <code>url(r'^i18n/', include('django.conf.urls.i18n'))</code> - This is what Django uses to set the language of the user and is necessary for URL calls to change it.</li>
</ol>
That should be it.  If there is anything I missed I'll be sure to update this post.
