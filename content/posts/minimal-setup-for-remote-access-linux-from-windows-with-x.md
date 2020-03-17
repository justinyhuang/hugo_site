---
title: Minimal Setup for Remote Access Linux from Windows with X
date: 2013-03-21T18:18:00
---

When accessing a Linux machine remotely in Windows, many might use VNC or similar remote desktop tools. Some might simply use Putty. Both are not quite satisfying: VNC and the like ask for too much network bandwidth when transferring graphical data, while Putty doesn't support X and limits your use to only in the console.

<!--more-->

Some might suggest using Cygwin, which will give you access to the X environment without sacrificing much bandwidth (it transfers the graphical parameters instead of pixel value). But still installing Cygwin is a pain, not to metion its huge size, which could be over 1G easily.

Recently I just realize there is a way to get to work remotely from Windows with X setup, and here is you could do it:

   *  Download a Putty client (I would recommend [KiTTY](http://kitty.9bis.net/), give it a try)
   *  Download and install [XMing](http://sourceforge.net/project/xming/files).
   *  Start XMing and then run your Putty client. Enable "Enable X11 forwarding" in Category->Connection->SSH->X11.
   *  Start a Putty session and try "xclock"

YES! It works! Even better, with minimal space and setup =)
