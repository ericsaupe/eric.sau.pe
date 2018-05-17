---
templateKey: blog-post
title: Getting Started With PostgreSQL
date: '2014-07-17T10:34:00-07:00'
tags:
  - postgresql
  - postgresql create
  - postgresql create database
  - postgresql database
  - postgresql docs
  - postgresql installation
---
PostgreSQL is a pretty common database choice these days. It's quick and fairly easy to set up but a lot of the information I've seen online makes it much harder than it has to be so I'm going to put my notes here.
<ol>
	<li><a href="http://www.postgresql.org/download/" target="_blank">Download PostgreSQL</a> and follow the installation guide for your flavor</li>
	<li>Fire up your console and type <code>$ psql</code> to open the Postgres command line
<ol>
	<li>From here you should do all of the commands you find online</li>
	<li>Everything you find on <a href="http://www.postgresql.com/docs/" target="_blank">http://www.postgresql.org/docs/</a> will work in the command line</li>
	<li>This will be your main interaction between you and your PostgreSQL databases</li>
</ol>
</li>
	<li>Create a Database - <code>CREATE DATABASE my_new_database</code></li>
	<li>Read the docs and do some Google searches to find all of the commands you'll need. There are way too many to list here and they all have their uses.</li>
</ol>
That should get you up and running. If you are doing a web development project your web framework would probably take over from here once you have your databases created. Django would create tables on its own, especially with South or 1.7 with migrations. Ruby on Rails could create tables using <code>$ rake db:create</code> or for more developed projects <code>$ rake db:schema:load</code>.

As usual if there is anything I've missed or questions that you have feel free to leave a comment below.
