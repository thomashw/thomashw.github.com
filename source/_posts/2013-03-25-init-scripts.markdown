---
layout: post
title: "init scripts"
date: 2013-03-25 19:26
comments: true
categories: 
---
When creating an init script, make sure the lock file it creates in `/var/lock/subsys` has the same filename as the init script. When switching run levels the rc scripts check for the existence of a file in `/var/lock/subsys` with the same name as the script, so if the lock file has a different name than the script the service won’t start/stop correctly even if there are start and kill symbolic links in the /etc/rc*.d directories.

I recently created an init script that used a different name for its lock file and it took me quite a while to figure out the issue. Oops.
