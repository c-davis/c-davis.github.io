---
layout: post
title: A simple Samba server on a Raspberry Pi, done the right way
---
So there are many pages with instructions on setting up a Samba server on a Raspberry Pi running Raspbian, just [let me google that for you](http://lmgtfy.com/?q=samba+server+on+raspberry+pi).

  > However I was yet to found one that actually works.

My set up consisted of a single HDD formatted in exFat, which I'll be using as the storage pool.

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

Now we need we add support for exFat drives
{% highlight js %}
sudo apt-get install exfat-utils -y
{% endhighlight %}

Next up is mounting the drive to the directory we made earlier..
{% highlight js %}
sudo mount -o uid=pi,gid=pi /dev/sda1 /mnt/usbstorage
{% endhighlight %}

The drive will not automount on start up, so lets do something about that!
{% highlight js %}
sudo ls -l /dev/disk/by-uuid/
{% endhighlight %}

This is what I will see. Copy the UUID.
{% highlight js %}
total 0
lrwxrwxrwx 1 root root 15 May 19 20:17 13d368bf-6dbf-4751-8ba1-88bed06bef77 -> ../../mmcblk0p2
lrwxrwxrwx 1 root root 15 May 19 20:17 15CD-3B79 -> ../../mmcblk0p1
lrwxrwxrwx 1 root root 10 Jan  1  1970 <strong>ECAC-3580</strong> -> ../../sda1
{% endhighlight %}








