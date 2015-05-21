---
layout: post
title: A simple Samba server on a Raspberry Pi, done the right way
---
So there are many pages with instructions on setting up a Samba server on a Raspberry Pi running Raspbian, just [let me google that for you](http://lmgtfy.com/?q=samba+server+on+raspberry+pi).

  > However I was yet to found one that actually works.

My set up consisted of a single HDD formatted in exFat.

First things first; SSH into our Pi our update our repositories!
{% highlight js %}
sudo apt-get update
{% endhighlight %}

We now need to make a directory
{% highlight js %}
sudo mkdir /media/USBHDD1
{% endhighlight %}

This will make the owner of the directory pi
{% highlight js %}
sudo chown -R pi:pi /mnt/usbstorage
{% endhighlight %}



