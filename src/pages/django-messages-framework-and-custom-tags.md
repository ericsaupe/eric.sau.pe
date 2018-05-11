---
templateKey: blog-post
title: Django Messages Framework and Custom Tags
date: '2013-08-22T11:10:00-07:00'
tags:
  - bootstrap messages fix
  - css
  - django
  - django bootstrap
  - django bootstrap fix
  - django fix
  - django messages framework
  - html
  - messages
  - messages framework
---
![null](/img/screenshot-from-2013-08-22-090436-300x144.png)

Sometimes it's necessary to display a message to the user in a quiet and clean manner.  To do this, Django has given us the <a title="Django Messages Framework" href="https://docs.djangoproject.com/en/dev/ref/contrib/messages/" target="_blank">messages framework</a> for cookie- and session-based messaging.  After a form or other input is submitted we can quickly send a short message to the user without much effort at all.  Here are all the <a title="Django Messages Framework" href="https://docs.djangoproject.com/en/dev/ref/contrib/messages/" target="_blank">documents</a> related to the framework but I'm going to give you the quick version as well as some customization options and Bootstrap fixes.

The messaging framework is enabled by default when you create a new Django project so good job you're half way there!  Now just add this bit of code to the top of any page you want messages to appear:

<pre>{% if messages %}
&lt;ul&gt;
    {% for message in messages %}
    &lt;li{% if message.tags %}{% endif %}&gt;{{ message }}&lt;/li&gt;
    {% endfor %}
&lt;/ul&gt;
{% endif %}</pre>

We have this in our base that is inherited by every page to allow the functionality to appear on the entire site.  You need to have the loop even if you only have one message because it helps clear the message storage needs to be cleared.  Now the great thing is you don't need to return messages when you render your template it will just come with it because of the context processor involved.

To add messages to your page, in your view:

<pre><code>from django.shortcuts import render_to_response
from django.template import RequestContext
from django.contrib import messages

def index(request):
    messages.add_message(request, messages.SUCCESS, 'We did it!')
    return render_to_response('my_app/index.html', context_instance=RequestContext(request))</code></pre>

That's it!  You just have to have the line <code>messages.add_message(request, MESSAGE_TYPE, MESSAGE_STRING)</code> and then use the <code>context_instance</code> in your render and you're done.

<h3>Bootstrap Compatability</h3>
To use this with Bootstrap you need to leverage the message.tags dictionary that returns values for each of the message levels.  Here is our code that we use for messages in our Bootstrap site.
<pre><code>&lt;div id="messages"&gt;
    {% if messages %}
        {% for message in messages %}
            &lt;div class="alert alert-{{ message.tags }}"&gt;
                &lt;button type="button" class="close" data-dismiss="alert"&gt;&amp;times;&lt;/button&gt;
                {{ message }}
            &lt;/div&gt;
        {% endfor %}
    {% endif %}
&lt;/div&gt;</code></pre>
As you can see we surround the message in an alert class with an <code>alert-{{ message.tags }}</code> class and add the Bootstrap close button.  This works out of the box for all Django messages except <code>messages.ERROR</code>.  This is because <code>messages.ERROR</code> returns 'error' which will make the alert class <code>alert-error</code> which is not what Bootstrap uses, they use alert-danger.  To fix this, as well as <a title="Django Message Tags" href="https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-MESSAGE_TAGS" target="_blank">add new message types</a>, you need to alter your settings file by adding this variable:
<pre><code>from django.contrib.messages import constants as message_constants
MESSAGE_TAGS = {message_constants.DEBUG: 'debug',
                message_constants.INFO: 'info',
                message_constants.SUCCESS: 'success',
                message_constants.WARNING: 'warning',
                message_constants.ERROR: 'danger',}</code></pre>
The import just above the line helps to avoid circular imports.  At this point you can see that we have changed the constant for <code>messages.ERROR</code> to return 'danger' instead of 'error'.  This fixes our Bootstrap problem but you can see that now you can add new message constants not already created like <code>messages.AWESOME</code> or whatever else you'd like to make.

There is a lot more to the Django messages framework including error levels that I didn't get into here.  Check out their docs for the low down on all the different things you can do with Django messages.

Here is a screenshot of an example I did just to test things out.

![](/img/screenshot-from-2013-08-22-090947-300x134.png)
