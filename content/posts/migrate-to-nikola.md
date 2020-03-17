---
title: Migrate To Nikola
date: 2015-03-10T21:29:05
---

After about one year since I put on the only post in 2014, eventually I write a new one: the summary of the year (so
shame for this only one...)

And then I realize Octopress, after upgrading to the latest version, doesn't work for my repo any more...Because I know
nothing about Ruby, this becomes absolutely a heart attack.

<!--more-->

After a few tries I decided to give up: I don't like Octopress in the first place. It is just only the first available
tool that I found for static blog generation. It worked, but not now.

Later Nikola appears in my search result. For someone feels more comfortable in Python. Nikola becomes my next straw of
rescue.

There are already many resources showing how to install, generate and deploy your blog/website with Nikola. This post is
by no means the 1001th instruction. I would just share what I feel about this static website generation tool:

* If you are familiar with Python, getting familiar with Nikola takes a second. From its configuration file to an
  exception when running the tool, you will feel just like home.

* This is more of my personal opinion: the reStructuredText syntax appears to be easier and the source file looks more
  organized. Compare the way you embedded a link in Markdown and reStructuredText and you will know what I am talking
  about.

* Some features, like defining the site for different localizations, are great. I am not aware of such handy features in
  Octopress. It is said that Nikola comes with a lot of plugin as well. I will try later.

* There are still too few themes that are available for Nikola. To the time of writing, there are only less than 10
  different looking themes available on the official site of Nikola. The current theme that I am using looks OK, but I
  would not use it if I have better choice, and yes, I am too lazy to build my own...

* The deployment of a Nikola site to Github is a myth. Some of the instructions from Google are out-of-date, while some
  new ones just don't work. Eventually I decided to go to the mailing list of Nikola, which is where I will recommend to
  you as well, and find out a way to successfully deploy the generated site.

While I am not 100% happy with Nikola, I would still recommend it to people who are looking for a way to generate static
webpages. It's free, under active development with considerable size of community.

Let's just hope it grows and gets better.


