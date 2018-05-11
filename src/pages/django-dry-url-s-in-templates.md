---
templateKey: blog-post
title: Django DRY URL's in Templates
date: '2013-08-27T11:18:00-07:00'
tags:
  - django
  - django 1.5
  - django template
  - django template url
  - django urls
  - dry
  - template url
  - url resolver
  - urls
---
![](/img/urls-300x164.png)

With so many templates in a large project that all link together in creative and complicated ways it can be a pain to keep track of URLs.  Every URL is an address that can also have a number of variables associated with it.  If these need to be changed it can be very tedious to go through every file and replace the hard-coded URLs to reflect the change.  Helping with this, Django has a <a title="Django Template URLs" href="https://docs.djangoproject.com/en/dev/ref/templates/builtins/#s-url" target="_blank">template feature</a> that will look those URLs up for you and insert the relative URL, without the domain name, into your templates on render.

It's a waste to have hard-coded URLs in your templates and if you are adhering to the DRY principle, don't repeat yourself, you'll not want to have the same hard-coded URL in many places in the same project.  Using the <code>{% url %}</code> tag you can reference a view and no matter what its URL is Django will handle it for you.

Example time:

<h3>urls.py</h3>
<pre><code>urlpatterns = patterns('marketing.views',
    url(r'^$', 'index'),
    url(r'^overview/$', 'overview'),
    url(r'^overview/(?P&lt;cus_id&gt;\d+)/$', 'overview'),
)</code></pre>
<h3>views.py</h3>
<pre><code>@staff_member_required
def overview(request, cus_id=None):
    if not cus_id:
        messages.add_message(request, messages.INFO, _('Please select a customer.'))
        return redirect('marketing.views.index')
    customer = Customer.objects.get(customer_number=cus_id)
    return render_to_response('marketing/overview.html', {'customer':customer,}, context_instance=RequestContext(request))</code></pre>
<h3>base.html</h3>
<pre class="lang:default decode:true ">&lt;!--This is the top menu for everything in this application--&gt;
{% block navbar %}
    &lt;li {% block nav-marketing %}{% endblock %}&gt;&lt;a href="{% url "marketing.views.index" %}"&gt;{% trans "Marketing Home" %}&lt;/a&gt;&lt;/li&gt;
    {% if customer %}
        &lt;li {% block nav-overview %}{% endblock %}&gt;&lt;a href="{% url "marketing.views.overview" customer.customer_number %}"&gt;{% trans "Overview" %}&lt;/a&gt;&lt;/li&gt;
        &lt;li&gt;&lt;a href="{% url "customers.views.customer" customer.customer_number %}"&gt;{% trans "Profile" %}&lt;/a&gt;&lt;/li&gt;
    {% endif %}
{% endblock %}</pre>
This application, Marketing, uses a base.html page that is extended in all sub pages.  This gives the ability to have the same menu across all pages in the app.  I also limit the links based on whether a customer has been selected or not.  Let's focus on the url tags though.  In the menu we see there are three links; marketing home, overview, and profile.  The first url tag demonstrates the most basic use of the Django url tag.  Simply it in your href and you have a link.  Make sure to have quotations around the view you are referencing, this was an update starting with Django 1.5.

Some URLs take parameters and we will need to pass those on occasion.  This is very easy to do by putting a space after the view and then adding the parameters, in order, with spaces between them.  If you don't want to use the default ordering of the URL you can call out the variable name in the url tag.  Changing the marketing overview link to use the named parameter it would change to <code>{% url "marketing.views.overview" cus_id=customer.customer_number %}</code>.  Simple enough, right?  You can even reference views used outside of the immediate app like I have done with the Profile link that moves you to the customers application.  Django will handle all of your URL needs.

Hope this clears up how to leverage this great tool from Django.  I know when I first started with Django I didn't use this as much as I should have but I'm loving it now.
