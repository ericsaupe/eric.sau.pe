---
templateKey: blog-post
title: 'Django Security Advisory: ImageField Abuse'
date: '2013-12-20T15:16:00-08:00'
tags:
  - django
  - django imagefield
  - django imagefield security flaw
  - django imagefield security issue
  - django security
  - django security flaw
---
A couple of weeks ago it came out that there is a flaw in Django's ImageField which could potentially allow for phising programs to be uploaded and grab cookies or do other malicious things.  While there will be no fix in Django directly you still need to take precautions on how you serve and receive files uploaded by your users.

Django has a page dedicated to fixing this exact issue.  Head over to their <a title="Django Security Page: User Uploaded Content" href="https://docs.djangoproject.com/en/dev/topics/security/#user-uploaded-content" target="_blank">security guide</a> and read up on the fixes.  They shouldn't be too hard and shouldn't affect any user experience.

Just wanted to post and let everyone know of the vulnerability.
