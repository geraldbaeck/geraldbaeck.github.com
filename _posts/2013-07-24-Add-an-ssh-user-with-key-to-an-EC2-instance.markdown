---
layout: post
title:  "Add an ssh user with key to an Amazon EC2 instance"
date:   2013-07-24
categories: devops
tags: ssh security
---
{% include JB/setup %}

First you need to create a pair of keys on your local machine (replace "user" with your chosen username):
{% highlight bash %}
ssh-keygen -b 1024 -f user -t dsa
{% endhighlight %}

This will create 2 files: user (private key), user.pub (public key). Now copy the public key file to a temporary place on your instance:
{% highlight bash %}
scp -i root *.pub ec2-your-instance-name.compute.amazonaws.com:/tmp
{% endhighlight %}

Log in to the instance as root. For each user you are creating, add the user to your instance with the
{% highlight bash %}
sudo adduser user
{% endhighlight %}

For simplicity's sake, use the same "user" name as you did for key generation. Now we need to place the key into their ssh authorized keys file (replacing "user" with the username you chose earlier)
{% highlight bash %}
sudo mkdir ~user/.ssh
sudo cat /tmp/user.pub >> ~user/.ssh/authorized_keys
sudo chmod 700 ~user/.ssh
sudo chmod 600 ~user/.ssh/authorized_keys
sudo chown user:user ~user/.ssh
sudo chown user:user ~user/.ssh/authorized_keys
{% endhighlight %}

Now log in:
{% highlight bash %}
ssh -i ~/.ssh/user -l user ec2-your-instance-name.compute.amazonaws.com
{% endhighlight %}

To add your new user to the sudoers list:
{% highlight bash %}
sudo adduser user sudo
{% endhighlight %}

Don't forget, that you probably want to delete an old user:
{% highlight bash %}
sudo userdel -r olduser
{% endhighlight %}