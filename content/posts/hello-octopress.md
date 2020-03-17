---
title: Hello Octopress
date: 2012-10-06T23:56:00
libraries:
- katex
---

This weekend I decided to migrate this blog from Wordpress to Octopress, because of many reasons searchable in google, and because with Octopress now I can write posts anywhere with Vim and Git. To help those who are now exploring Octopress (and myself sometime later), I've made some notes on the obstacles that I've seen as well as the solutions:

<!--more-->

**Migration from Wordpress to Octopress**

The migration was pretty easy, once you export from Wordpress to an XML file, [exitwp](https://github.com/thomasf/exitwp) generates the markdown files for you with a few key strokes. The only thing lost during the migration is the comment, but your life would made easy if you are going to use [Disqus](disqus.com):

**Migration of Comments**

To move your comments from Wordpress to Octopress, first install the Disqus plugin on your Wordpress and then export the comments into Discus.
It is said that the import might take up to 24 hours but I guess it will be much shorter for many of us.
After the import completes, go to \[your account\].disqus.com/admin/tools/migrate/, build a CSV file following the example and upload the file for URL mapping. That is it for comment migration. Easy~

**Customization**

The next step is customization. There are not too many themes available for Octopress when I setup this site, and it appears that I have two options if I need something different:

   1. follow the instructions @ the [official site](http://Octopress.org/docs/theme/template)
   2. get the [Slash theme](http://zespia.tw/Octopress-Theme-Slash/#overview)

As you could see I am too lazy to hack and therefore just go for option 2.

**Localization Support**

For bloggers who write posts in Chinese, the current Octopress has a limitation: lack of Chinese category support. There are also two ways out:

   1. To name the categories under 'category_dir' in Chinese codings like RFC1738. [This method](http://geron.heroku.com/blog/2012/03/octo-cate-cn-spo) works when I preview the pages locally, but failed after deploying the changes to Github.
   2. To name the categories in english, but display the nick names (in Chinese or other localizations). [The second method](http://blog.sprabbit.com/blog/2012/03/23/Octopress/) asks for not only changing the category_generator.rb, but also some of the framework files. It works perfectly locally and on the server side. I would recommend this approach.

**LaTeX Support**

Even though Octopress claims itself as *A blogging framework for hackers*, LaTeX is not supported by default. To blog with some mathematics, you could try the following to obtain this capability:

1. in _config.yml, change

{{< highlight yml >}}
        markdown: rdiscount
{{< /highlight >}}

    to

{{< highlight yml >}}
        markdown: kramdown
{{< /highlight >}}

2. install kramdown by doing

{{< highlight bash >}}
        gem install kramdown
{{< /highlight >}}

   in bash

3. in source/_include/custom/head.html add the code below:

{{< highlight html >}}
   <script type="text/javascript"
       src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
   </script>
{{< /highlight >}}

4. try the following

{{< highlight latex >}}
    R_{ab} - {\textstyle 1 \over 2}R\,g_{ab} + \Lambda\ g_{ab} = \kappa\, T_{ab}
{{< /highlight >}}

and you should see

$$R_{ab} - {\textstyle 1 \over 2}R\,g_{ab} + \Lambda\ g_{ab} = \kappa\, T_{ab}$$

