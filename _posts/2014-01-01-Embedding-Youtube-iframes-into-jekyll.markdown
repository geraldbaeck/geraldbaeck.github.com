---
layout: post
title:  "Embedding Youtube iframes into Jekyll"
date:   2014-01-01
categories: sysadmin
tags: mac xcode
---
{% include JB/setup %}

With Xcode 5.0.1 and Mavericks the Xcode command line tool is no longer available through Xcode. You need to install it manually. You need the command line tools to compile stuff or for example to run Mac Ports.

There are many ways to install the command line tools. If you are running OS X 10.9 the command line is your friend:

{% highlight bash %}
xcode-select --install
{% endhighlight %}

You can also do it via XCode itself. Just go to Preferences > Downloads.

And finally download it manually at [https://developer.apple.com/downloads/index.action](https://developer.apple.com/downloads/index.action).

