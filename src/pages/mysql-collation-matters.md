---
templateKey: blog-post
title: MySQL Collation Matters
date: '2013-07-25T12:09:00-07:00'
tags:
  - django
  - mysql
  - utf8
---
When creating databases and tables with MySQL there seems to be one setting that gets overlooked, collation and character set.  For most smaller sites, and maybe those purely in English, this may not be such a huge deal but for a larger application site and one that will be holding characters from various languages this is essential.

UTF-8 is the collation and character set we want and the MySQL command to create a database with these settings is <code>CREATE DATABASE dbname CHARACTER SET utf8 COLLATE utf8_general_ci;</code>

Bringing this back to Django, when Django creates new tables within a database it will use whatever the default character set and collation is for that database.  This is why it is important to get this right at the beginning with the command above.  If you do not, however, there are ways to make future tables created have the right encoding.  Simply add these lines at the bottom of your database connection settings in your settings.py file:
<pre><code>
'OPTIONS': {
'init_command':'SET storage_engine=INNODB,
                character_set_connection=utf8,
                collation_connection=utf8_unicode_ci'
}</code></pre>
I put mine right below the PORT variable in my default database connection.  This will ensure that all new tables created will have the correct collation and character set.  But what about those tables already created that need their settings changed to accommodate these special characters?  You can find out what collation the columns are using by entering this command for MySQL: <code>SHOW FULL COLUMNS FROM my_table</code>.  One of the columns shown is the Collation column.  Make sure this is some utf8 encoding and you're good.  If it isn't simply use the command <code>ALTER TABLE my_table MODIFY column_name varchar(30) CHARACTER SET utf8;</code> to change the column to the appropriate character set.  Make sure that the varchar(30) in the example is set appropriately to your column whether it be a text field, varchar(255), etc. make sure it is the same column it was before just with the new character set.  Run that command for every column that needs changing.  Run the SHOW FULL COLUMNS line again and make sure everything is right and that's it!

Hope this helps someone, I know it has come up a lot in the project we are working on now and has become kind of a headache because we didn't start the tables out correctly.
