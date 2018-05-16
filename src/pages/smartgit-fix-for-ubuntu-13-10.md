---
templateKey: blog-post
title: SmartGit Fix for Ubuntu 13.10
date: '2013-10-23T15:03:00-07:00'
tags:
  - smartgit
  - smartgit fix
  - smartgit ubuntu
  - smartgit ubuntu 13.10
  - smartgit ubuntu 13.10 fix
  - smartgit ubuntu fix
---
After updating from Ubuntu 13.04 to 13.10 I found that my <a title="SmartGit" href="http://www.syntevo.com/smartgithg/" target="_blank">SmartGit</a> was broken.  I could just use the standard git commands but that's boring and I like to have a GUI so SmartGit is nice for that and it needed to be fixed.  Apparently <a title="Ubuntu 13.10 SmartGit Help" href="http://askubuntu.com/questions/362920/smartgit-crash-in-ubuntu-13-10" target="_blank">Ubuntu 13.10's GTK libraries aren't very compatible with SmartGit</a>, any version of it.  The fix is simple.
<ol>
	<li><code>sudo nano smartgithg.sh</code></li>
	<li>Add the line <code>UBUNTU_MENUPROXY=0</code> just underneath the comments at the very top of the file.</li>
	<li>Save.</li>
</ol>
That's it!  Fixed.  Hope this helps.  I'll update when there is an official patch.  In the mean time this works fine.

UPDATE

They just released RC1 which fixes the problem.  <a href="http://www.syntevo.com/smartgithg/early-access?file=smartgithg/smartgithg-generic-5-rc-1.tar.gz">Download SmartGit 5 RC1 here</a>.
