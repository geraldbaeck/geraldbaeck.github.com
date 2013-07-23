---
layout: post
title:  "Synchronize your ssh configuration across multiple machines with Dropbox"
date:   2013-07-23
categories: devops
tags: ssh dropbox
---
{% include JB/setup %}


Working with many different Macs and servers daily can ba a pain. But you can store your SSH configuration file in dropbox, and create a symbolic link to it so you can use the same configuration across your computers.

Also, you can use this method to sync other types of configuration files like your bash profile or your hosts file.

Create a folder in your Dropbox to store such files.

{% highlight bash %}
mkdir ~/Dropbox/symlinks
{% endhighlight %}

Move your ssh config to this folder.

{% highlight bash %}
sudo mv /etc/ssh_config ~/Dropbox/symlinks/ssh_config
{% endhighlight %}

Create a symbolic link to the new file.

{% highlight bash %}
sudo ln -s ~/Dropbox/symlinks/ssh_config /etc/ssh_config
{% endhighlight %}
