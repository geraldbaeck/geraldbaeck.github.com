---
layout: post
title:  "Configure your favorite ssh hosts to type less"
date:   2013-07-23
categories: devops
tags: ssh
---
{% include JB/setup %}

ssh is pretty straight forward. I spend years typing long lines into the console or setting up complicated UI to manage my favorite ssh hosts. But there are some tricks, which could save you a lot of time.

At first, you need to create an ssh config file. If you are on a mac the file is located at /etc/ssh_config. All other unix-like machines should create the file in ~/.ssh/config. To check where the OS is looking for a config file you could also type 

{% highlight bash %}
ssh -v test
{% endhighlight %}

It should return something like this:

{% highlight bash %}
OpenSSH_5.9p1, OpenSSL 0.9.8x 10 May 2012
debug1: Reading configuration data /etc/ssh_config
{% endhighlight %}

Make sure the file has read-write permission to only your user.

{% highlight bash %}
chmod 600 /etc/ssh_config
{% endhighlight %}

Now, configure a your favorite hosts in the ssh_config file

{% highlight bash %}
Host example
HostName example.com
Port 667
User username
IdentityFile ~/.ssh/id_rsa

{% endhighlight %}

Now, instead of writing this:

{% highlight bash %}
ssh -i ~/.ssh/id_rsa -p 4431 username@example.com 
{% endhighlight %}

You can write this:
{% highlight bash %}
ssh example
{% endhighlight %}

As a small benefit this also works with scp.

{% highlight bash %}
scp /path/to/some/file example
{% endhighlight %}

Now let's tunnel. If you are used to write this, to make a connection to a remote mySQL database

{% highlight bash %}
ssh -f -N -i ~/.ssh/id_rsa -L 9906:127.0.0.1:3306 username@database.example.com
# -f puts ssh in background 
# -N makes it not execute a remote command 
{% endhighlight %}

You should consider modifying your config like this:
{% highlight bash %}
Host tunnel
HostName database.example.com
IdentityFile ~/.ssh/id_rsa
LocalForward 9906 127.0.0.1:3306
User username
{% endhighlight %}

To be able to finally establish a tunnel with this command:
{% highlight bash %}
ssh -f -N tunnel
{% endhighlight %}
