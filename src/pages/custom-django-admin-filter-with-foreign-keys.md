---
templateKey: blog-post
title: Custom Django Admin Filter with Foreign Keys
date: '2013-06-20T11:43:00-07:00'
tags:
  - admin
  - django
  - django admin
  - filter
  - foreign key
  - mysql
---
During our massive redesign of our internal tools I wanted to make the Django admin page more friendly for us, the software engineers and admins.  Currently when they have problems or need things changed the easiest way to do that is through raw SQL and altering the database directly.  This is inconvenient and can lead to bad data.  Django, by design, abstracts that interaction and creates a great user-interface to interact with including filters and search options.

One of the strengths of this is you can define your own custom filters that will appear as normal filters on the UI.  Defining them is quite simple and you can check out the <a href="https://docs.djangoproject.com/en/dev/ref/contrib/admin/">tutorial directly from Django</a>(Search for DecadeBornListFilter).  I wanted to take the query a step further and have it look up items based on a characteristic of a parent object stored as a foreign key.  Using the double underscore (__) Django allows you to access data from that foriegn key in queries.  Following the tutorial I was able to quickyl make my query.

It worked great, except when the query was set to All.  When set to All it should display every object for that queryset because there are no parameters but since I was using foreign key fields I guess it didn't like that and would display nothing.  When you selected specific filters the query worked but on the default nothing appeared.  I searched all over the Internet and found nothing about it so I made up my own solution.

<pre><code>def queryset(self, request, queryset):
     #Without this check the All button would fail
     if self.value():
          return queryset.filter(test__category=self.value())
     return queryset</code></pre>
<div>My solution is to check whether or not there is a value.  On All there is no value and should return the queryset unchanged.  Before it would try to filter and return nothing.  This simple test made it so my admin page has a lot more functionality and was a simple fix that I couldn't find written anywhere.  Hopefully this helps someone else through the odd behavior that can happen sometimes.</div>
