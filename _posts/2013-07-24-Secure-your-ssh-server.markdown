---
layout: post
title:  "Secure your ssh server"
date:   2013-07-24
categories: devops
tags: ssh security denyhosts
---
{% include JB/setup %}

While ssh is pretty secure, your should consider making it even more secure, by adding some of these steps.
* [Use keys instead of passwords](#disablekeys)
* [Disable login as root](#disableroot)
* [Install denyhosts](#denyhosts)
* [Change your ssh port](#changesshport)

*Caution: Whenever making changes to your sshd.config file, be certain that you have an active shell session in case you've made a fatal syntax error. After restarting sshd, log in from another session to test it before terminating your active terminal session.*

###Passwords are bad - use keys{#disablekeys}
Passwords are always a kind of obscure security mechanism. A better and certainly more secure mechanism is to use ssh keys for all users. If ypu want to know how to generate a new user with his own key, have a look at [this guide]({% post_url 2013-07-24-Add-an-ssh-user-with-key-to-an-EC2-instance %}).

To disable Password-based Login log in to your instance and edit the ssh daemon configuration file:
{% highlight bash %}
sudo nano /etc/ssh/sshd_config
{% endhighlight %}

Find the line
{% highlight bash %}
PasswordAuthentication yes
{% endhighlight %}

and change it to
{% highlight bash %}
PasswordAuthentication no
{% endhighlight %}

Don't forget to restart sshd:
{% highlight bash %}
sudo service ssh restart
{% endhighlight %}

###Disable Root Login{#disableroot}
One of the biggest security holes you could open on your server is to allow directly logging in as root through ssh, because anyone can attempt to brute force your root password. It’s much better to have a separate account that you regularly use and simply sudo to root when necessary. You should make sure that you have a regular user account and that you can su or sudo to root from it.

We need to edit the sshd_config file, which is the main configuration file for the sshd service.
{% highlight bash %}
sudo nano /etc/ssh/sshd_config
{% endhighlight %}

Find this section in the file, containing the line with “PermitRootLogin” in it. Set it to "no" to disable logging in through ssh as root.
{% highlight bash %}
PermitRootLogin no
{% endhighlight %}

And finally restart your ssh service
{% highlight bash %}
sudo service ssh restart
{% endhighlight %}

###Install denyhosts{#denyhosts}

DenyHosts is a security tool that monitors server access logs to prevent brute force attacks It works by banning IP addresses that exceed a certain number of failed login attempts. 

DenyHosts is very easy to install:
{% highlight bash %}
sudo apt-get install denyhosts
{% endhighlight %}

After you install DenyHosts, make sure to whitelist your own IP address. Skipping this step will put you at risk of locking yourself out of your own machine. 

Open up the list of allowed hosts allowed on your server:
{% highlight bash %}
sudo nano /etc/hosts.allow
{% endhighlight %}

Under the description, add in any IP addresses that cannot afford to be banned from the server; you can write each one on a separate line, using this format:
{% highlight bash %}
sshd: 12.34.45.678
{% endhighlight %}

After making any changes, be sure to restart DenyHosts so that the new settings take effect on your virtual private server:
{% highlight bash %}
sudo service denyhosts restart
{% endhighlight %}

DenyHosts is ready use as soon as the installation is over, but if you want to customize DenyHosts, you can make the changes within the DenyHost configuration file:
{% highlight bash %}
sudo nano /etc/denyhosts.conf
{% endhighlight %}

###Change your ssh port{#changesshport}

There are many [discussions](http://serverfault.com/questions/189282/why-change-default-ssh-port) on the web about why changing your ssh from the default 22 to something else is a good or bad idea. Personally I think it is a good thing, because it adds a small security layer to the system. It is just like keeping your phone number out of the white pages. It won't someone who really wants to call your doing that, but it will keep the kiddies away. 

Changing your port has some upsides:
* Brute Force attack on port 22 are useless (Script Kiddies)
* It reduces the impact on your log files

And also some downsides:
* It can be a pain to explain other users that they can't use the default port and how to change that
* Some restrictive outgoing firewalls are blocking non standard ports and only allow connections to for example 22, 25, 80, 443

I think if a port change causes you even some trouble, it is probably not worth the effort because there are far more effictive ways to protect your server.

If you do move away from 22, make sure it is below 1024. Under most Unix-a-like setups in their default config, only root users can listen on ports below 1024, but any user can listen on the higher ports. Running ssh on a higher port increases the chance of a rogue (or hacked) user managing to compromise your SSH daemon. You can find a list of known port numbers [here](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)

So how to do it? Just edit the line "Port 22" in your /etc/ssh/sshd_config file
