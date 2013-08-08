---
layout: post
title:  "Python if else one liner"
date:   2013-08-08
categories: python
---
{% include JB/setup %}

Instead of:
{% highlight python %}
def func(x):
    if x == 0:
        return "something"
    else:
        return "something else"
{% endhighlight %}

Just do this:
{% highlight python %}
def func(x):
    return "something" if x == 0 else "something else"
{% endhighlight %}

Nice to know and saved 3 lines of code!-)