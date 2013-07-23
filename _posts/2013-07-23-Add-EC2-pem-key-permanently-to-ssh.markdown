---
layout: post
title:  "Add EC2 pem key permanently to ssh"
date:   2013-07-23
categories: devops
tags: ssh pem
---
{% include JB/setup %}

Add your pem key to SSH so you do not have to manually specify it when connecting to EC2 instances.

With this command:
{% highlight bash %}
    ssh-add ~/.ssh/KEY_PAIR_NAME.pem
{% endhighlight %}

You can do this:
{% highlight bash %}
    ssh ec2-instance.amazonaws.com
{% endhighlight %}
instead of:
{% highlight bash %}
    ssh -i ~/.ssh/KEY_PAIR_NAME.pem ec2-instance.amazonaws.com
{% endhighlight %}
