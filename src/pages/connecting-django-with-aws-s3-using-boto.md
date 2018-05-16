---
templateKey: blog-post
title: Connecting Django with AWS S3 using Boto
date: '2013-12-02T15:13:00-08:00'
tags:
  - amazon
  - amazon aws
  - amazon s3
  - aws
  - aws boto
  - aws django
  - aws python
  - aws s3
  - aws s3 boto
  - aws s3 boto django
  - aws s3 boto python
  - boto python
  - boto s3
  - django boto
---
Amazon Web Services (AWS) has been revolutionary in getting servers and processing power to virtually anyone on the cheap.  You can host your web site there, process large sets of data, even back up your personal files to an Amazon S3 server.  Even if your site isn't hosted directly on AWS you can still benefit by hosting your files from there.  A lot of sites do this to help off load the bandwidth costs onto Amazon because in many cases it is cheaper than the bandwidth costs for your site.  In this post I want to go over how to access these files stored on an S3 instance of AWS from Python using a plugin known as <a title="Boto" href="http://boto.readthedocs.org/en/latest/index.html" target="_blank">Boto</a>.  You can read the <a title="Boto S3" href="http://boto.readthedocs.org/en/latest/ref/s3.html" target="_blank">Boto S3 commands</a> at their website.  There are a lot of them and I am only going over the basics here.

Boto is a Python interface to Amazon Web Services and helps you interact with every aspect of AWS.  I'm just going to focus on the S3 portion since that is where we store files for clients.  We designed a way for clients to view and store files through our client portal.  It is sort of like Dropbox but not nearly as sophisticated.  We are able to drop files in their folder for them to view and they can even add in files for themselves.  Everything is saved back at Amazon and everyone is happy.  To accomplish all of this in our Django project we needed to interact with S3 as if it were a normal file system.  Since S3 is remote, connections need to be made, files need to be uploaded, and paths need to be verified.  One of the easiest things we did was create a FileUtils class that handled all of our Boto code and then simply created an instance of it and called our methods.  Let's go through our code and feel free to change whatever you need to to make it work for you.  I've stripped out a lot of the more proprietary code that is uninteresting or unneeded in the course of this post.

<em>There was a ton of code and this post was messy so I moved it all to a <a title="AWS S3 Python Boto FileUtils Example" href="http://pastebin.com/2iZ0PBcz" target="_blank">PasteBin</a> link.</em>

Let me explain everything that is happening here.  Because we are storing files specific to clients we need to keep them separated.  We gave them the ability to share documents through email addresses.  You'll notice throughout that we make checks for FileSharesPermissions to verify the user has access to the file they are trying to access.  If you do not care about that and you just want to serve files, feel free to rip all of that out.

The FileUtils class is created by passing in a companyID.  Again, if you are not worried about separation of files just remove all of that.  What is important is a rootFolder, s3Connection, and s3Bucket.  We have opted to place these in the settings file as they will be used and changed as needed.  The rootFolder can be anything you want it to be.  S3 stores files using a key name that can be equated to a path to a file.  Inside of your bucket you will have keys organized by their name and sub categorized by their name just like a path to a file.  We have set up a rootFolder where we will be placing client files separated by their client ID.  Once again, if you don't need to separate then drop all that code and stick the files wherever you'd like.

In the FileUtils class you'll see a number of very basic file manager methods.  Save, create folder, delete, get file, get raw file, move, move file, get contents, and various helper methods to accomplish all of this.  Most of the methods should be self explanatory and are only complicated by the check for permission to access the file.

A few things of note.  Everytime you want to save a new file you need to create a new key using Boto.  You'll see that under the save_file method that k = self.s3Bucket.new_key(path).  This is creating a new empty key, no data, at that name in the S3 instance.  It then sets its contents from a string, passed from a file upload, and is saved.  You do not need to worry about creating a folder or subfolder for the file.  S3 takes care of that using the name of the key.  For example, if I have a totally empty S3 instance and I try to save a file to Files/New Files/Some Other Folder/My PDF.pdf all I would need to do is set the name of the key that holds My PDF.pdf as that path name.  S3 will automatically read the slashes as folders and take care of the rest.  This makes things tricky for something like creating a new folder.  Something actually has to be in the folder to be considered by S3 to be a folder.  To get around this you'll see under the create_folder method that we generate a key at that path and set the contents to nothing.  This creates the key, S3 displays it as a folder, but that folder is technically empty.  Now you can see it and save files to it whenever you'd like.

When you retrieve a file S3 generates a temproary URL.  Under the method get_file you'll see that we return the url = k.generate_rul(120).  The 120 is how many seconds that temporary URL will be valid for.  This is great for security as it requires users to constantly regenerate the URL to ensure they still have access to it.  The get_raw_file on the other hand does not use the temporary URL and instead just gets the contents of the file and returns it to the user.  Both methods work and you should use the one that suits your needs.

This post might be really complicated and a bit hard to understand.  If anything needs to be clarified please feel free to comment below and I will be glad to rewrite and expound more on the code and workings of Boto.
