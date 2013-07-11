---
layout: post
title:  "Configuring Wifi on your Raspberry Pi"
date:   2013-07-11 08:33:58
categories: rapsberrypi
tags: rapsberrypi wifi linux
---
{% include JB/setup %}

There are many ways how to configure wifi on the Raspberry Pi. Most of them are complicated and messy. I found this solution the easiest to apply. One important thing is, that this will not work with a hidden wifi. You will always have to broadcast your network's ssid.

Connect to you Pi via ssh and type

{% highlight bash %}
sudo nano /etc/network/interfaces
{% endhighlight %}

You should see something like this

{% highlight bash %}
auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp
{% endhighlight %}

Edit it this way

{% highlight bash %}
auto lo

iface lo inet loopback
iface eth0 inet dhcp

auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-ssid "YOUR_NETWORK_SSID_HERE"
wpa-psk "YOUR_WIFI_PASSOWRD_HERE"
{% endhighlight %}

Finally unplug your network cable, reboot your Pi and check ifconfig if you have an active wifi connection.

{% highlight bash %}
sudo reboot
ifconfig
{% endhighlight %}

ifconfig should return something like this. The important thing is to see an assigned IP address in the wlan0 section.

{% highlight bash %}
eth0      Link encap:Ethernet  HWaddr b8:27:eb:95:4d:5d
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

wlan0     Link encap:Ethernet  HWaddr 80:1f:02:97:4e:c2
          inet addr:10.0.1.6  Bcast:10.0.1.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:3692 errors:0 dropped:3978 overruns:0 frame:0
          TX packets:402 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:356570 (348.2 KiB)  TX bytes:55122 (53.8 KiB)
{% endhighlight %}

**Troubleshooting**
- Is your network not hidden?
- It does not work without a password
- Are the password and the ssid with double brackets?
