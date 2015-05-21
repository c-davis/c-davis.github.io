---
layout: post
title: A simple Samba server on a Raspberry Pi, done the right way
---
So there are many pages with instructions on setting up a Samba server on a Raspberry Pi running Raspbian, just [let me google that for you](http://lmgtfy.com/?q=samba+server+on+raspberry+pi).

  > However I was yet to found one that actually works.

My set up consisted of a single HDD formatted in exFat, which I'll be using as the storage pool.

##First things first, mounting our drive!

SSH into our Pi to update our repositories
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

We'll now change the fstab to automount on start up
{% highlight js %}
sudo nano /etc/fstab
{% endhighlight %}

At the very bottom add this, replacing the UUDID I've entered with your own.
{% highlight js %}
UUID=<strong>ECAC-3580</strong> /media/USBHDD1 exfat   uid=pi,gid=pi   0   0
{% endhighlight %}

Save your fstabd now reboot your pi!
{% highlight js %}
sudo reboot
{% endhighlight %}

##Next up is the Samba side

We need to install samba first, when prompt enter Y
{% highlight js %}
sudo apt-get install samba samba-common-bin
{% endhighlight %}

We now need to edit our smb.conf file like we changed our fstab
{% highlight js %}
sudo nano /etc/samba/smb.conf
{% endhighlight %}

The only changes you need to make are 
{% highlight js %}
workgroup = WORKGROUP
wins support = yes
{% endhighlight %}

Staying in smb.conf, we need to share the directory we made earlier.
{% highlight js %}
[All the space in the world]
 comment=Share
 path=/media/USBHDD1
 browseable=Yes
 writeable=Yes
 only guest=no
 create mask=0777
 directory mask=0777
 public=no
{% endhighlight %}
 
Now, I have users on the network who I don't want access to this share. Hence "public=no" which means the share will require credentials to access.
{% highlight js %}
sudo smbpasswd -a pi
{% endhighlight %}
Then set the password to raspberry.

We're all done with Samba so I suggest restarting it
{% highlight js %}
sudo /etc/init.d/samba restart
{% endhighlight %}

Once restarted you can now test your share on the next work.
<table>
  <thead>
    <tr>
      <th>Domain</th>
      <th>Username</th>
      <th>Password</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>raspberrypi</td>
      <td>pi</td>
      <td>raspberry</td>
    </tr>
  </tbody>
</table>

 
 
 





