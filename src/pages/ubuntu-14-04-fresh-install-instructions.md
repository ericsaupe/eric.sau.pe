---
templateKey: blog-post
title: Ubuntu 14.04 - Fresh Install Instructions
date: '2014-04-17T10:33:00-07:00'
tags:
  - ubuntu
  - ubuntu 14
  - ubuntu 14.04
  - ubuntu django
  - ubuntu git
  - ubuntu jdk
  - ubuntu jdk 8
  - ubuntu mysql
  - ubuntu pip
  - ubuntu pycharm
  - ubuntu virtualenv
---
With the latest LTS release of <a title="Ubuntu" href="http://www.ubuntu.com/" target="_blank">Ubuntu</a> many people, including myself, will take this time to do a fresh install of the OS on their development machines.  Here are a few tips for getting up and running after your OS is installed.  This guide will be for Python and Django development using Ubuntu 64-bit, alter the commands to work with your flavor of Ubuntu.
<h2>Git</h2>
<ol>
	<li><code>sudo apt-get install git-core</code></li>
</ol>
<h2>JDK</h2>
<ol>
	<li>Download the latest version of <a title="Oracle JDK 8" href="http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html" target="_blank">JDK from Oracle</a>.</li>
	<li><code>cd /usr/local</code></li>
	<li><code>sudo tar xvzf ~/PATH/TO/DOWNLAD/xvzf jdk-8u5-linux-x64.tar.gz</code></li>
	<li><code>sudo nano /etc/profile</code></li>
	<li>At the bottom of this file paste the following lines:</li>
</ol>
<pre style="color: #000000;"><code>JAVA_HOME=/usr/local/jdk1.8.0_05
PATH=$PATH:$JAVA_HOME/bin
export JAVA_HOME
export JRE_HOME
export PATH</code></pre>
In the terminal type <code>java</code> to make sure it is working.
<h2>PyCharm</h2>
If you want to use settings from your previous install of PyCharm click <code>File-&gt;Export Settings</code> and back up this file to be used on your new installation.
<ol>
	<li>Download the latest version from <a title="PyCharm Download" href="http://www.jetbrains.com/pycharm/download/" target="_blank">PyCharm</a>.</li>
	<li><code>cd ~/PATH/TO/DOWNLOAD/</code></li>
	<li><code>tar xvzf pycharm-professional-3.1.2.tar.gz</code></li>
	<li><code>cd pycharm-3.1.2/bin/</code></li>
	<li><code>./pycharm.sh</code></li>
	<li>If PyCharm asks if you want to use old settings say that you don't right now, we will do this after it is installed.</li>
	<li>Follow PyCharm instructions to finish installation</li>
	<li>Now you can import the settings by clicking <code>File-&gt;Import Settings</code> and locating the files.</li>
</ol>
<h2>Pip</h2>
<ol>
	<li><code>sudo apt-get install python-pip</code></li>
</ol>
<h2>VirtualEnv</h2>
<ol>
	<li><code>sudo pip install virtualenv</code></li>
	<li><a title="Developing with Virtualenv" href="http://ericsaupe.com/developing-with-virtualenv/" target="_blank">Create your virtualenvs.</a></li>
</ol>
<h2>MySQL</h2>
<ol>
	<li><code>sudo apt-get install python-dev</code></li>
	<li><code>sudo apt-get install mysql-server</code></li>
	<li><code>sudo apt-get install libmysqlclient-dev</code></li>
	<li>In your related virtualenv run <code>pip install MySQL-python</code></li>
</ol>
<h2>Django</h2>
<ol>
	<li>In your related virtualenv run <code>pip install Django</code></li>
</ol>
At this point you should be ready to install all of the other packages you need.  If I've missed anything critical for getting an Ubuntu installation at least up to a working state let me know below in the comments.
