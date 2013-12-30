---
layout: post
title:  "Hosting a Jekyll Blog on Github"
date:   2013-12-30
categories: sysadmin
tags: jekyll blogging
---
{% include JB/setup %}

For this blog and some others I use Jekyll and github to run. This has many advantages. Github cares about the hosting and scaling stuff and even the deployment is only a git push away. 

Jekyll is a static blogging engine written in ruby. I do not know shit about ruby, but it simply works very well.

To run your own Jekyll blog on Github just follow the steps outlined [here](http://jekyllbootstrap.com/usage/jekyll-quick-start.html).

To make it run on your custom domain, just create a file called CNAME in the root directory of your repository and just put your custom domain insdie it.

Finally the above only describes hosting a blog on github pages for one account, but you can do this for as many accounts as your wish. Just create a new repository and push your jekyll data to the branach "gh-pages" like I do it [here](https://github.com/geraldbaeck/eve.baeck.at/tree/gh-pages).
