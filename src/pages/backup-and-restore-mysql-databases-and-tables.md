---
templateKey: blog-post
title: Backup and Restore MySQL Databases and Tables
date: '2013-09-05T11:28:00-07:00'
tags:
  - mysql
  - mysql backup
  - mysql remote backup
  - mysql remote restore
  - mysql restore
---
Anytime I need to move data or alter our database I get a little bit nervous that something is going to go wrong and crash the database losing all of our data, dropping tables, causing chaos, and so many phone calls.  Our database automatically backs up every so often but right before any major changes I make a quick local copy of the data.  This came in handy just the other day when a table was inadvertently dropped and I had a backup on hand.  Here's how to do it using console commands.
<h3>Making a Backup of a Database</h3>
<pre class="lang:mysql decode:true ">mysqldump -u [username] -p --host=[hostname] [database_name] &gt; [output_name].sql</pre>
<h3>Restoring a Database from a Backup</h3>
Make sure the database is created before restoring.
<pre class="lang:mysql decode:true">mysql -u [username] -p --host=[hostname] [database_name] &lt; [output_name].sql</pre>
<h3>Making a Backup of a Table in a Database</h3>
<pre class="lang:mysql decode:true">mysqldump -u [username] -p --host=[hostname] [database_name] [table_name] &gt; [table_name].sql</pre>
<h3>Restoring a Table from a Backup</h3>
Make sure the table has been created already and MySQL will know where to put the data.
<pre class="lang:mysql decode:true">mysql -u [username] -p --host=[hostname] [database_name] &lt; [table_name].sql</pre>
<h3>MySQL error 1449: The user specified as a definer does not exist</h3>
To fix this problem use the following command when restoring your database.
<pre class="lang:mysql decode:true  crayon-selected">mysqldump --single-transaction -h [hostname] -u [username] -p [database_name] &gt; [output_name].sql</pre>
If you rename columns you will get some weird results so make sure that the tables are the same before and after the backup. Let me know about experiences or tips you have with backing up and restoring MySQL databases.
