---
templateKey: blog-post
title: Django and South - Making Your Data Migration Easier
date: '2013-10-04T14:57:00-07:00'
tags:
  - data migration
  - django
  - django mysql south
  - django south
  - django south data migration
  - mysql
  - south
  - south mysql
---
Django works wonderfully as a web framework but sufferes from sub-par data migration.  Luckily for all of us one of their developers has created an application called <a href="http://south.aeracode.org/" target="_blank">South</a> that fixes this very problem.  South is meant to bring data migrations to Django and make it easy to make schema changes that are quickly applied to your database without much effort on your part.  Django does have a few built in features for dropping tables, creating tables, and will give you the code to enter to create the tables but it doesn't help much when you are working on a production server that you don't want to simply drop and readd tables all the time.

To get started just go through the <a href="http://south.readthedocs.org/en/latest/installation.html" target="_blank">installation tutorial</a>.  Once you've done that you're ready to migrate your data!  This is best done from the very creation of the project so South can help you from the beginning.  If you are starting a new project <a href="http://south.readthedocs.org/en/latest/tutorial/part1.html" target="_blank">go here</a> and follow along.  I'm going to focus a bit more on converting an already created project and making updates and changes to your projects.

Now that South is installed on your already created project you've been using for a while you'll need to convert your app so South can interact with it.  In your console just enter the commands:
<pre><code>./manage.py syncdb
./manage.py convert_to_south myapp</code></pre>
Done!  Now your app is ready to go with South.  South keeps track of the migrations using a few tables in your database which is why you need to syncdb before converting any of your apps.  After you have converted your apps you'll notice you have some new .py files that will need to be uploaded to your repositories.  When South tries to do a migration it makes sure that you have all of the .py files that it has listed in its database for that app.  After the initial conversion, and you've uploaded the files, all other machines with the installation will need to run the command ./manage.py migrate myapp 0001 --fake to ensure everyone starts on the same page.  The --fake command at the end tells South to take note of what is happening but don't actually do it because it's already been done.

When you make changes to your models, from now on, you'll need to use South to do the syncdb instead of Django.  Let's say you add a few columns to a table, remove some others, rename even more, and add a bunch of new tables.  To have South take care of all of that just use the commands:
<pre><code>./manage.py schemamigration myapp --auto
./manage.py migrate myapp</code></pre>
After running the first command South will try to document all of the changes, display them to the screen, and if all goes well it will tell you to run the second. When running the second command it will actually apply the changes to the database. That's all there is to it. Occasionally, something will go wrong. If a problem occurs at the first command it's most likely because you have a problem in your code that is making it fail to compile. If it fails in the second command there are more issues. You could have a partial migration which they will give you code to try and clean up but I've found it's best to do it yourself as their solution is usually to drop a lot of tables. It's kind of a headache when that happens and has turned off a few of us from South but for the most part we have had a lot of success with it. If you do make all of the changes by hand just be sure and tack on the --fake at the end of the second command to let South know that the changes were already made and that they shouldn't try and do them again. Be sure and always commit your .py South files because even if I am in a different app trying to use South I will need all of the South .py files from all of the apps before the sync can happen.

Hope this helps a few people and explains a few things about the nuances of South. It really is a great tool and they are even trying to work it into the default installation of Django in the near future so that will be exciting. If you have any tips or tricks when using South post them in the comments below.
