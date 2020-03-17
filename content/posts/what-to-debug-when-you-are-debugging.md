---
title: What to Debug when You Are Debugging
date: 2013-09-13T10:52:00
---

![debug](/galleries/what_to_debug_when_you_are_debugging.gif)

Debugging is probably the first task for most of us in practice: Day 1 when you joined the team, someone senior called you to his desk and gave you a defect description, "go and figure out what goes wrong, young boy".

<!--more-->

Since then you have a feeling that debugging would be with you for the rest of your programming life.

And a majority of software engineering is about debugging, too. Therefore it is really worth spending some time considering: What to debug when you are debugging? Well I have to say the title is too big to fit in the post, however, considering everyone would have his/her own checklist when debugging, I feel like sharing mine here though it is far from complete.

Let's start from the origin of life: what is debugging? Google will point you to a [formal definition](http://en.wikipedia.org/wiki/Debugging), while I just understand it as 'to find the bug in the code, and fix it'. Knowing this one could easily start debugging by asking: Is there a bug? How do you know?

A way to reproduce the bug is a valid proof. Also don't forget to check against the design document and be sure the behavior caused by the 'bug' is not as designed. It is not uncommon that a test engineer throw a defect to you, which turns out to be a designed feature. So, first step, "**make sure there is a bug and it is really a bug**".

Debugging to software engineers is pretty much the same as tracking down criminals to detectives. A murder case will never be solved if no one knows who was killed, where and about when the victim was murdered. Similarly, we will need clues to get to the bug: normally a trace/log file, or a consistent way to recreate the issue. It would drive me crazy if someone asks me to find a bug, with only "it crashes and I have to power off the machine. There is no log file and I don't how to reproduce it."

Some might argue they can find a bug by literally reading the code, but hey, do you really think that is efficient or even practical? I would not suggest trying this in any real world project.  That is the second on the checklist: "**either find a way to reproduce the issue, or get a good enough log file**".

It becomes more interesting when we know it is a real bug and some clues are available. If what you get is a lengthy log file, then what you need is a quiet cube, put on your "do not disturb" plate and try to catch the bug in the lines. The easiest way is to look for keywords like "error", "warning" or the assert lines, but you don't always get the luck.

Sometimes you would need to figure out the clues by putting the puzzles together, from the big log file in front of you, which sometimes could be in several G's. When you get to this point what you need is a good tool to help you filter out the unuseful information and show you the gold. Use whatever tool you like the best, and if you don't have one yet, you might like to try [mine](/posts/repeat-in-vim): filtering in [Vim](http://www.vim.org) is powerful.

In short, "**get your tools work for you on the log files**".

If the defect is reproducible, that is good news: the bug has been trapped and it is just a matter of time to locate and fix it. Debugging with a debugger, or repeat testing with more verbose debug output enabled, will reveal more information to where the bug hides. When debugging on codes that you are not familiar with, "**talk to people who might either give you hints or correct your misunderstandings about how the code should be running**". Believe me, it will save you tons of time.

It would be your lucky day if you see another revision of the code running correctly, or the same code running on a different platform without the issue: you can simply compare the two different behaviors and quickly locate the bug!

"**Looking for suspicious**"

- Cache

    One thing you should check is the cache.

    There is one case where I set a value in the memory via DMA. What I saw in Snoop (a tool used to listen to the traffic on the data/address bus) is that memory has been written with the value by DMA, but after the write if I try to read the memory, I still get the old value. But if I wait for a while, the value I read becomes the new one.

    This is because the read, in this case, is performed on the cached value. Only when the cached page gets updated do I read the new one.

    Two possible solutions here:

   - invalidate/flush the cache very time when you need to read the most recent value.

     This will ensure all the cached values to be updated, but will introduce considerable overhead. Also if some of the other values do need caching, updating these values might cause some side effects.


   - access the non-cached copy of the memory.

     There are platforms that support two mapped addresses pointing to the same memory: one in the physical memory and the other in cache. Simply access the non-cached memory one could always get the most recent value.
     For instance, in PHX (an OS used in one of my past jobs), memory in 0x01234567 is the cached memory, while 0x41234567 is the non-cached memory. The address 'OR' 0x40000000 will point you to the non-cached memory.

- Timing
     Especially in multi-thread/process environment, the execution flow might not be the same as you thought.


- Analog v.s Digital
     Do check the signal level is high enough to be read as a '1'


- Stack Information
     When the system crashed, data in the stack of each running thread will be the only evidence to tell you what has happened.


- Hardware Issue
     It would be wise to tell whether it is a hardware issue or a firmware defect: sometimes it won't hurt to swap some hardware and redo the test. Instead of wasting your time debugging something doesn't exist, let's first make sure the hardware is correctly working...
     One example, that cost me about 2 days, is that when working on AOAO (AutoOnAutoOff) there is a defect in which the device would crash when wake up from ActiveOff. By looking at the trace it appears the PC pointer somehow gets messed up, likely to be a memory corruption. So I kept on debugging and could not find out who stirs the water...until Mike says "let's swap our formatter and see if we could reproduce the issue". Later on it is obvious that the issue goes with the hardware: some of the DRAM chips would flip the data at certain situation, and the hardware team is still investigating this hardware issue: which took me 2 days trying to find its firmware counterpart, it just doesn't exist!
