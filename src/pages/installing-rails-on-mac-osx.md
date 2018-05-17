---
templateKey: blog-post
title: Installing Rails on Mac OSX
date: '2014-07-08T10:34:00-07:00'
tags:
  - rails
  - rails install mac osx
  - rails installation
  - rails mac osx
  - ruby
  - ruby on rails
---
When developing for Ruby on Rails the first thing you'll need to set up your development environment. This guide will be for installing things on Mac OSX and would be similar for Linux. These are my notes from experience setting up a Rails environment based on <a href="http://www.railstutorial.org/" target="_blank">The Rails Tutorial</a> by Michael Hartl.

New Rails Installation on OSX
<h3>Install Git</h3>
Checks if git is installed and if it isn’t it will install it
<pre> $ git</pre>
<h3>Install Ruby (Already installed)</h3>
Checks Ruby version, already installed on OSX
<pre> $ ruby -v</pre>
<h3>Install RVM</h3>
RVM is for multiple Ruby on Rails environments for different projects.
<pre> $ curl -sSL https://get.rvm.io | bash -s stable</pre>
<h3>Install RVM Requirements</h3>
<pre> $ rvm requirements</pre>
NOTE: RVM tries to install gcc46 which is an older version of gcc on OSX. Mavericks already has gcc installed so install the list that appears except gcc. It also can install but take a VERY long time so just be patient and see.
<h3>Tell RVM where OpenSSL is</h3>
<pre> $ rvm install 2.0.0 --with-openssl-dir=$HOME/.rvm/usr</pre>
<h3>Create .gemrc</h3>
This file stops the installation of Ruby docs, the text below goes in .gemrc
<pre> install: —no-rdoc —no-ri
 update: —no-rdoc —no-ri</pre>
Install Rails
<pre> $ gem install rails</pre>
You should be ready to create your environment and install your Gems. If I've missed anything, or you have problems, let me know in the comments below and I will update this list.
