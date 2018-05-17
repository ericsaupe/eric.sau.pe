---
templateKey: blog-post
title: GitLab - Your Own Private GitHub
date: '2014-04-11T10:31:00-07:00'
tags:
  - bitnami
  - bitnami gitlab
  - bitnami gitlab aws
  - bitnami gitlab ec2
  - git
  - github
  - github vs gitlab
  - gitlab
  - gitlab aws
  - gitlab ec2
  - gitlab vs github
---
![](/img/gitlab_0.png)

<a href="https://github.com/" target="_blank">GitHub</a> is great for open source projects and if you haven't been there to see what's going on you're missing out.  They offer free git hosting for all of your open source projects.  Their UI is great for tracking issues, commits, users, updates, and so much more.  But let's say you want to use all of that great UI on a project you don't want open source.  That's when you'll need to pay GitHub for a few private repositories.  Or maybe you want to create an organization for you and your developers to create private projects and manage larger applications across many repositories.  You'll need to pay for that too. 

Those prices can get pretty steep.  Luckily for you there is an open source solution to GitHub called GitLab.  It runs on Ruby and you can set it up all by yourself by following these <a href="https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md" target="_blank">GitLab Installation Instructions</a>.  That assumes you have a server ready to serve and work for you.  If you don't you can quickly set up a great Amazon EC2 instance and <a href="https://aws.amazon.com/marketplace/pp/B00CFPZ9SG" target="_blank">install GitLab in one click</a>.  There are a couple of caveats in getting things set up with the EC2.  Once it is installed you'll log in using the account user@example.com and the password is contained in your EC2 log.  Once logged in change your password to something other than the auto created one.  You'll then need to make some changes to your config files.  SSH into your EC2 instance and update the URL contained in <code>~/apps/gitlab/htdocs/config/gitlab.yml</code>.  Follow the instructions at the top of that file.  Then you'll need to set up your email service.  To do that <a href="http://wiki.bitnami.com/Applications/BitNami_GitLab#How_to_configure_the_email_settings_of_GitLab.3f" target="_blank">follow the instructions here</a> and then restart the services as described at the bottom of that section.  Now you're all set!  Placing this in a free Amazon instance will give you a server for a year for free and then after will only cost ~$15 per month.  Much cheaper than GitHub and you have complete control to create groups, private repositories, even public repositories!

That's all there is to it.  I've started using it for my personal projects that I want a UI for issue tracking and other users to join me with and not make public.  If you have any great advice for using GitLab feel free to share in the comments below.
