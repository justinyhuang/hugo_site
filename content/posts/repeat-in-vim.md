---
title: Repeat in Vim
date: 2013-08-06T18:48:00
---

There are many ways to repeat something in Vim. Knowing how and when to repeat would greatly save your time with Vim.

<!--more-->

**Get Started**

Natively Vim provides some basic methods to repeat:

   * the '.' will repeat the latest operation: if you 'dd' to delete a line, '.' will continue deleting another
   * the 'p' pastes contents copied (or in the Vim language, yanked) before. Combined with the use of [registers](http://vimdoc.sourceforge.net/htmldoc/change.html#registers), one can keep multiple copies of different content and pastes them into other places.

**Be More Powerful**

Besides the basic toys, Vim enables much powerful repeat technique with its record feature, the [q](http://vimdoc.sourceforge.net/htmldoc/repeat.html#recording) command allows to repeat a series of Vim operations, which, when nested or with numbers of repeated operations specified, could be extremely efficient. A life changing feature.

Example:
When analyzing logs it is sometimes required to remove the time stamps and compare two logs.
There are many ways to remove time stamps in a log file, using the recording feature is one of them.
In the example below, I first go into the recording mode, remove the timestamp of the first line, go into the next line, and end recording. The remove-time-stamp operation has been recorded and stored into register 'q'. Because there are still 48 lines to be processed, I simply type '48@q' to repeat the same operation for 48 times and the whole log is cleaned.

![repeat in vim 1](/galleries/repeat-in-vim-1.gif)

Not everyone would think of it but regular expression is another technique that could be used to repeat operations in Vim. And when you used to it, you will give up anything to be able to do regexp in Vim =)
There are many resources available for using regular expression (Google is your best friend...), there is even a [website](vimregex.com) just for this topic! I think it would not be very interesting for me to 'repeat' here...

**Make Extra Steps**

Besides the native support provided by Vim, there are many plugins available out there to further improve your efficiency.
[Marvim](http://www.vim.org/scripts/script.php?script_id=2154) being one of them, makes it easier to store your frequently-used-and-complicated operations and even easier to repeat them.

Marvim allows saving operation macros by name and groups the stored macros by namespace. On the other hand Marvim provides searching the operations by name: especially helpful when you have a lot of operations saved.
A simple but illustrative example is shown below for those who are interested.

![repeat in vim 2](/galleries/repeat-in-vim-2.gif)
