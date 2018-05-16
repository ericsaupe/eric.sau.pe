---
templateKey: blog-post
title: Accessing a Git Repository Using a Key Pair
date: '2014-01-29T15:31:00-08:00'
tags:
  - 'Error: Permission denied (publickey)'
  - git
  - git aws
  - git ec2
  - git keypair
  - git permission denied
  - git publickey
---
We recently moved our git repositories over to Amazon Web Services.  We ran into one issue with it which was that now our git requests needed to have a key pair attached.  The Internet was not very kind on explaining how to do this very well so I'm documenting it here.

The key pair you'll want to use is the one generated for you by AWS.  It is the same one you use when you ssh into your AWS EC2 instances.  Take this key pair file, which I will call keypair.pem from now on, and we'll need to move it and configure our git to use it.
<ol>
	<li>Copy the keypair.pem into your .ssh folder.
<pre class="lang:default decode:true crayon-selected">cp /path/to/file/keypair.pem ~/.ssh/keypair.pem</pre>
</li>
	<li>Create/open your .ssh config file.
<pre class="lang:default decode:true">nano ~/.ssh/config</pre>
</li>
	<li>In your config file
<pre class="lang:default decode:true">Host git
Hostname EC2PublicDNS
User ubuntu
IdentityFile /home/username/.ssh/keypair.pem</pre>
</li>
	<li> To clone the repository use the following
<pre class="lang:default decode:true">git clone git:path/to/projects/PROJECT_NAME.git</pre>
</li>
	<li>To update existing projects on your machine to use the new address follow the instructions below</li>
	<li>Open your project's git config file
<pre class="lang:default decode:true">nano ~/path/to/project/.git/config</pre>
</li>
	<li>Change the url line to be
<pre class="lang:default decode:true">git:path/to/projects/PROJECT_NAME.git</pre>
</li>
</ol>
That's it. Hope this helps anyone who is getting the error 'Error: Permission denied (publickey)'.  There are other ways to do this but this way was pretty easy.
