---
layout: post
title: "VirtualBox Shared Folders"
date: 2012-09-01 10:05
comments: true
categories: 
- virtualbox
---
Last night I was trying to get shared folders working using VirtualBox. I have the host as Ubuntu 12.04 and a guest using Ubuntu 11.04. For the life of me I couldn't get them to work. I followed the instructions on installing the Linux Guest Additions from VirtualBox's site and set up shared folders the same way many tutorials/blogs suggested, but every time I went to mount the shared folder I would get this error:
    /sbin/mount.vboxsf: mounting failed with the error: No such Device

I *finally* found a post explaining that the issue was the installation of modules while installing the Guest Additions. VirtualBox's [website](http://www.virtualbox.org/manual/ch04.html#idp18411760) instructs to install *just* dkms before running the Guest Additions script, but this was incorrect. One indication that this is the issue is to run  `ls mod | grep vbox`. You **shouldn't** see vboxsf.

In order to build the modules necessary for shared folders, the following three packages need to be installed before running the Guest Additions installation script:      

    sudo apt-get install dkms build-essential linux-headers-generic      

After that, I re-ran the Guest Additions script by connecting the VBoxGuestAdditions.iso as a storage device (using phpvirtualbox), mounting the device at /dev/scd0, and running the script:

    sudo mount /dev/scd0 /media/cdrom
    cd /media/cdrom
    ./VBoxLinuxAdditions.run

Once that had completed, I restarted and ran `ls mod | grep vbox`. You **should** see see vboxsf.

One that's done, you should be able to mount your shared folders, like so:
    mount -t vboxsf -o uid=1000,gid=1000 d /media/sf_d

I put one of those commands for each of my shared folders in /etc/rc.local. Make sure you create the sf_d folder in /media before trying to mount it there, and change uid and gid to the user you want the mount to be mounted under.