---
templateKey: blog-post
title: '*args and **kwargs'
date: '2013-05-25T11:41:00-07:00'
tags:
  - args
  - computer science
  - kwargs
  - programming
  - python
---
As a recent graduate in computer science, I really probably should have known this, but today I learned.  This morning as the team and I were discussing some new methods and software to write the discussion came up about what were the <code>*args</code> and <code>**kwargs</code> we so often see in method declarations within Python.  Off to the Internet we went and found a <a title="*args and **kwargs?" href="http://stackoverflow.com/q/3394835/575985" target="_blank">perfect explanation</a> from none other than StackOverflow.

Turns out <code>*args</code> and <code>**kwargs</code> are arguments given to a method.  Now, we all knew this to be true, this was not the interesting part.  The interesting thing to me was that you can put these in your method declarations if you want it to accept varying lengths of arguments rather than explicitly defined ones.  Very cool indeed.

Now, I've used arguments a lot in computer science especially command line arguments when writing small assignments for classes.  I knew these were passed in as <code>*args</code> but was unsure of what the <code>**kwargs</code> were.  From the StackOverflow answer they are both sets of arguments except the difference is that <code>*args</code> is an ordered list of values while <code>**kwargs</code> is a dictionary key-pair or key word arguments.  So passing in either a list or a dictionary will allow us to use these variables to allow our methods to take any number of arguments.

Probably should have known that.  Probably should have a deeper and better understanding of it.  I'm just getting started.  You learn something new everyday.

-Eric
