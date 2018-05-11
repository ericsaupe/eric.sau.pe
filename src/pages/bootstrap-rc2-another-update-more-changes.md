---
templateKey: blog-post
title: 'Bootstrap RC2 - Another Update, More Changes'
date: '2013-08-16T11:03:00-07:00'
tags:
  - bootstrap
  - bootstrap 3
  - bootstrap 3 rc2
  - bootstrap rc2
  - css
  - html
---
As Bootstrap nears releasing their final version of Bootstrap 3 they will continue to roll out Release Candidates for bug fixing and testing.  We have elected to keep as up to date with them as we can under the assumption that release candidates are fairly stable and shouldn't change too much between now and release.  Tuesday they released their next release candidate, RC2.  You can read about the full changes at their <a title="Bootstrap RC2" href="http://blog.getbootstrap.com/2013/08/13/bootstrap-3-rc2/" target="_blank">blog</a> but I'm going to highlight a few of the main things that changed for us.
<h2>Default Col is Gone</h2>
In RC1 we were using <code>col-</code> to be the default size for all devices.  RC2 has removed this default class.  Instead they have added a new <code>-xs</code> size for really small phones and a <code>-md</code>, though I think this was in RC1 we just never used it.  The best way to do a default size for <em>most</em> devices is now to use to <code>col-sm-</code>.  Use the small version because it works for devices with a screen resolution  of ≥768px.  This is what <em>most</em> modern devices are using.  Anything lower than that will default your spans to be like a <code>col-12</code>.  If you need to make changes for screens smaller than that use <code>col-xs-</code>.
<h2>Navbar Changes</h2>
Some little things changed with the navbar.  Just quick changes to classes.  You'll need to wrap the mobile navbar header in a div with the class <code>navbar-header</code>.  Change <code>nav-collapse</code> to <code>navbar-collapse</code>.
<h2>Large and Small or lg and sm?</h2>
To keep consistency throughout Bootstrap they have changed many of the button classes from <code>button-large</code> to <code>button-lg</code> and <code>button-small</code> to <code>button-sm</code>. Form groups is also affected by this change.

Those are some of the main things we found.  I'm sure there are plenty of things that are affecting other people.  Head over to their <a title="Bootstrap Blog" href="http://blog.getbootstrap.com/" target="_blank">blog</a> to read all about it.  I'll update this post if I come across anything else.
<h2>EDIT Aug 16 12:06PM</h2>
It seems we've stumbled upon a bug in their Modal.  When a modal is opened and then closed when you reopen it you can't close it with the x or the close button. They are aware of the issue.  Our modals are actually all out of whack and we are looking into that right now which is how we found that bug.
<h2>EDIT Aug 16 12:20PM</h2>
Modals loaded remotely are being loaded into the <code>modal</code> and not the <code>modal-body</code>.  To fix this just move all of the divs below the modal into your remote document and that should work for now.  <a href="https://github.com/twbs/bootstrap/issues/9459" target="_blank">It's an issue they are discussing</a>.

EDIT Aug 19 1:01PM

When you print it uses the <code>col-xs</code> tag.  Make sure to update accordingly.
