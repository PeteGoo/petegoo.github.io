---
layout: post
title: Moved blog to GitHub Pages and Jekyll
categories:
- blog
tags:
- jekyll
- GitHub
status: publish
type: post
published: true
meta: {}
author:
  login: petegoo
  email: pete@petegoo.com
  display_name: PeteGoo
  first_name: Peter
  last_name: Goodman
---
Like most of the bloggers on the internet these days I've moved my blog off wordpress and onto GitHub pages using Jekyll. 

There was a fairly large amount of [Yak Shaving](http://www.hanselman.com/blog/YakShavingDefinedIllGetThatDoneAsSoonAsIShaveThisYak.aspx) involved in this process. I'm not going to do a tutorial on how to move from Wordpress to Pages/Jekyll, you can find plenty of info on how to do that on the links below. I will however point out some of the things that threw me.

* [This is a good tutorial](http://hadihariri.com/2013/12/24/migrating-from-wordpress-to-jekyll/) by [Hadi Hariri](https://twitter.com/hhariri)
* Follow the [Jekyll migrations site](http://jekyllrb.com/docs/migrations/) for instructions on exporting the Wordpress XML. (The Wordpress plugin didn't work for me)
* Learn that Windows is a second class citizen in this toolset and you are going to have to shave some Yaks.
* Make sure you [install Ruby, Jekyll, Bundler](https://help.github.com/articles/using-jekyll-with-pages)
* Set up your GitHub pages repo.
* Choose a theme and pull it into your repo. Decide whether you are going to fork it or just pull it into your existing repo.
* When importing ignore [the Jekyll import site bash script](http://import.jekyllrb.com/docs/wordpressdotcom/) and just use `jekyll import wordpressdotcom --source wordpress.xml` instead
* Make sure to [add the correct encoding](https://github.com/PeteGoo/petegoo.github.io/commit/2f52eb963ad0ddc76242586c677bcaf300e72fa1) for your site if you are on Windows.
* Watch out for the problem with "&#123;&#123;" characters in your xaml. [Broken by Liquid 2.0](http://jekyllrb.com/docs/troubleshooting/). [Fixable by escaping](https://github.com/PeteGoo/petegoo.github.io/commit/36a553e74edbc18196a2d5989f00c594fe6bd010).
* Remember to keep changing the url in the _config.yml to suit your current deployment or strange things might happen.
* DO NOT waste time trying to get [FrontMatter defaults](http://jekyllrb.com/docs/configuration/) working. I couldn't and gave up. Wasted sooooo much time on this. Instead I just changed the template to always switch on Disqus comments on posts.
* Disqus support admitted that they currently have a problem with their import, hence my old comments are not there yet.
* Learn to get permalinks right and use the [jekyll-redirect-from plugin](https://github.com/jekyll/jekyll-redirect-from) which GitHub pages supports.

Good Luck!