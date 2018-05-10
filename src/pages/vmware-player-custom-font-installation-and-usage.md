---
templateKey: blog-post
title: VMware Player Custom Font Installation and Usage
date: '2013-07-09T12:05:00-07:00'
tags:
  - fonts
  - printing
  - vmware
---
Came across this one today and wanted to document it for future needs.  We had a machine running Windows 8 that needed a VM of Windows XP to print reports from an old version of Access.  Everything installed fine and after getting the printer detected on the guest OS using VMTools we ran into a problem where the custom fonts we installed weren't being used.  Turns out you need to have the custom fonts installed on both the guest and the main OS.  The guest OS uses the fonts to view the report but when you try and print the request goes up out of the guest OS and into the main OS where the file will be printed.  If the custom font is not on the main OS it will not be found and will print wrong.  Just a quick tip.
