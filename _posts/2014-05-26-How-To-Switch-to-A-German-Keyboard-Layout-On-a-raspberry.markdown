---
layout: post
title:  "How to Switch to a german keyboard layout on a Raspberry"
date:   2014-05-26
categories: raspberry
tags: raspberry, keyboard, linux, german
---
{% include JB/setup %}

Open up a shell and type

{% highlight bash %}
  sudo nano /etc/default/keyboard
{% endhighlight %}

find where it says

{% highlight bash %}
  XKBLAYOUT=”gb”
{% endhighlight %}

change it to:
{% highlight bash %}
  XKBLAYOUT=”de”
{% endhighlight %}

