---
templateKey: blog-post
title: Django Context Processors
date: '2013-07-24T12:08:00-07:00'
tags:
  - context processor
  - context processors
  - django
  - global variables
---
Today I started work on allowing users to edit certain settings we have for them, namely what office they are in and their language.  Using Bootstrap I changed the way our navbar worked so clicking on your name drops down to give you the option of editing your profile or logging out.  Editing your profile brings up a modal with the choices you can change.  This modal is located in our project's base.html file so that users can change their settings on any page.  Generating the form with the options was where this post came in.

For a while now I've been using Django's default language changing form.  In it they reference a variable LANGUAGES.  For the life of me I had no idea where that variable was being passed in from.  Now I needed my own variable, OFFICES, to be passed into every page as well.  After a quick Google search I found out that these variables are passed to the templates through context processors.  In your Django settings file you can see all of the context processors currently being used under TEMPLATE_CONTEXT_PROCESSORS.  Specifically, the one passing the LANGUAGES variable to all templates is django.core.context_processors.i18n.

To pass custom variables you'll obviously need a custom context processor.  I created my own context_processors.py file in my Django project folder and this is what it looks like.
<pre><code>
from employees.models import OFFICE_CHOICES

def extra_context(request):
"""
Add variables that you would like accessed across all pages.

Author:
     Eric Saupe
"""
context_extras = {}
context_extras['OFFICES'] = OFFICE_CHOICES

return context_extras
</code></pre>
And on the settings file under TEMPLATE_CONTEXT_PROCESSORS I added:
<pre><code>
'my_project.context_processors.extra_context'
</code></pre>
You need to make sure to reference the method defined in the context_processors file.  With that being done you are free to add that variable in any of your templates without worrying about passing it directly through the views.
