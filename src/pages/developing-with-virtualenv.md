---
templateKey: blog-post
title: Developing with Virtualenv
date: '2013-11-19T15:10:00-08:00'
tags:
  - multiple versions of django
  - multiple versions of python
  - python virtualenv
  - virtualenv
  - virtualenv install
---
When developing you'll occasionally need more than one version of Python, Django, etc. for your various projects.  The way to have multiple versions of the same library installed on one machine is to use <a title="Virtualenv" href="https://pypi.python.org/pypi/virtualenv" target="_blank">virtualenv</a>.  Virtualenv creates a virtual environment where you can install a unique version of Python, Django, etc.  This is great for testing and for working on old projects where you need old versions of things.  For example, our old project at work uses Django 1.4 but the new system uses Django 1.6.  I can't run the 1.4 project with 1.6 because of all the deprecated methods and changed folder structure.  Instead, I create a virtual environment for 1.4 and another for 1.6.  This way I load the Python modules required for one project differently than the other.  In this guide I want to show you how to quickly set up a new virtual environment and install all necessary Python libraries.

The first thing you'll need to do is install <a title="Virtualenv" href="https://pypi.python.org/pypi/virtualenv" target="_blank">virtualenv</a>.  The quickest way is by using the command <code>[sudo] pip install virtualenv</code>.

Once that's done you'll need to create a folder for your virtual environment.  For my Django 1.4 environment I made a local folder called Django1.4 using the command <code>mkdir Django1.4</code>.

Now simply run <code>virtualenv Django1.4</code>.

Activate the virtual environment by running <code>source Django1.4/bin/activate</code>.

And you're in!  This is running a fresh and clean virtual environment not linked to your other installations.  Now you can install any version of Django and all your other Python libraries and they will always be separate from your main installation or other virtual environments.  Note, do not use sudo in any of your commands as that will install the packages to the main installation and not just your virtual environment.

To stop the virtual environment run the command <code>deactivate</code> from anywhere.

Once you have Django installed and the version of Python that you'd like you're ready to go.  A good idea is to keep track of what version the packages you're using is.  Creating a requirements.txt file that contains a formatted list of Python libraries and their versions will make setting up new virtual environments even more painless.  Once you've setup and installed all necessary Python libraries to your new virtual environment you can export that list using the command <code>pip freeze < requirements.txt</code>.  This creates a text file containing all installed Python libraries.  When you, or anyone else, needs to install your necessary libraries you can run the command <code>pip install -r requirements.txt</code>.  This reads each line in the file and installs the correct version to that virtual environment.

I hope this helps as I'm sure plenty of people need multiple developing environments for testing and maintaining old code.  We will be using it in the future to see what major changes need to be made from our Python 2.6 code to be Python 3 compatible.  If you have any other tips or tricks with virtualenv please feel free to comment below.
