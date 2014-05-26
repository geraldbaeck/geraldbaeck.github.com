---
layout: post
title:  "How to Switch to a german keyboard layout on a Raspberry"
date:   2014-04-05
categories: raspberry
tags: raspberry, keyboard, linux, german
---
{% include JB/setup %}

Open up a shell and type

{% highlight shell %}
  sudo nano /etc/default/keyboard
{% endhighlight %}

find where it says

{% highlight shell %}
  XKBLAYOUT=”gb”
{% endhighlight %}

change it to:
{% highlight shell %}
  XKBLAYOUT=”de”
{% endhighlight %}

