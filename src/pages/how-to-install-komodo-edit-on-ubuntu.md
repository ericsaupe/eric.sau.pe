---
templateKey: blog-post
title: How to Install Komodo Edit on Ubuntu
date: '2013-10-21T15:01:00-07:00'
tags:
  - how to
  - how to update komodo edit
  - komodo edit
  - komodo edit install
  - komodo edit install ubuntu
  - komodo edit update ubuntu
  - komodo ide
  - komodo install ubuntu
---
My IDE of choice for developing lately has been <a title="Komodo Edit" href="http://www.activestate.com/komodo-edit" target="_blank">Komodo Edit</a>.  It's lighter than Eclipse and does what I need it to do.  Installing and updating on most OS's is easy but on my Ubuntu installation I always have to do a few more tricks to make everything work just right.  This post is to help me remember exactly the steps of installing and updating so that I don't need to Google it every time.
<ol>
	<li>Download the latest version of <a title="Komodo Edit Download" href="http://www.activestate.com/komodo-edit/downloads" target="_blank">Komodo Edit</a></li>
	<li><code>tar xvzf Komodo-Edit-8.5.2-13850-linux-x86.tar.gz</code> Update this to your downloaded version</li>
	<li><code>cd Komodo-Edit-8.5.2-13850-linux-x86</code></li>
	<li><code>sudo ./install.sh</code></li>
	<li>When prompted for installation location enter <code>/opt/Komodo-Edit-8/</code></li>
	<li>If you already have Komodo Edit installed in that location run <code>sudo rm -r /opt/Komodo-Edit-8/</code> to uninstall it first.</li>
	<li><code>export PATH="/opt/Komodo-Edit-8/bin:$PATH"</code></li>
	<li>Search for it in your Dash and all should be good.</li>
</ol>
Hope this helps someone else, I know it will help me.
