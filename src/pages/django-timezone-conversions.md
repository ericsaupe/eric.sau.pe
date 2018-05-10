---
templateKey: blog-post
title: Django Timezone Conversions
date: '2013-07-17T12:06:00-07:00'
tags:
  - conversion
  - django
  - pytz
  - timezone
  - timezone conversion
---
Django does a great job at converting datetime objects to whatever the servers timezone is when your settings are set to do so.  For our big project we needed times to be local to the user.  Django documentation says to allow the user to select their time zone and save it to their user profile, which we plan to do.  Until then, to get things working, a few tests were in order.

I have a page where a user can make a note about a company and a timestamp is created and displayed for when the note was made.  When the template is rendered it takes the UTC specific time saved on the database and converts it to our local server timezone of 'America/Denver' automatically.  To make the site more responsive, and less hits to the server, I make the creation of these notes happen through an AJAX call.  The information is then displayed on the list quickly and quietly.  The problem I was running into was that the JSON object returned in the AJAX call had the UTC time and not the local time for the user.  When the page was refreshed, Django took over and displayed it correctly but I didn't want to refresh the page every time.

Converting the timezone is pretty simple and here is what I found from the <a href="https://docs.djangoproject.com/en/dev/topics/i18n/timezones/#troubleshooting" target="_blank">Django documentation</a> altered to be specific to my one time zone change.

<pre><code>import pytz
local_tz = pytz.timezone(user.get_profile().timezone)
# This is the correct way to convert between time zones with pytz.
local_time =local_tz.normalize(note.date_created.astimezone(local_tz))</code></pre>

A few things to note are that the datetime object must be timezone aware and not naive.  If it is naive you will need to follow the Django documentation link to find how to give datetime objects timezones.  Hope that helped someone, I learned something.
