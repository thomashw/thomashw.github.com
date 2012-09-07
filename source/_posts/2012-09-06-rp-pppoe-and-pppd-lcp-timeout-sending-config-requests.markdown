---
layout: post
title: "RP-PPPoE &amp; pppd - LCP: timeout sending Config-Requests"
date: 2012-09-06 19:34
comments: true
categories: 
- pppoe
---
Recently I was installing RP-PPPoE 3.11 on CentOS 5.2 (Linux version 2.6). This will enable us to authenticate users connecting to our network through one of our computers (which I'll call 'server').

Whenever I tried to connect to the server from a client, I would see the following in /var/log/messages on the server:

	pppd[22137]: pppd 2.4.4 started by root, uid 0
	pppd[22137]: Using interface ppp0
	pppd[22137]: Connect: ppp0 <--> /dev/pts/1
	pppd[22137]: LCP: timeout sending Config-Requests 
	pppd[22137]: Connection terminated.
	pppd[22137]: Modem hangup
	pppoe[22140]: Timeout waiting for PADO packets
	pppd[22137]: Exit.

After doing a ton of digging, I finally read a tip to try disabling the syslog daemon. This tip didn't include why you would need to do this or what it had to do with RP-PPPoE, but I was running out of options so I gave it a try.

	sudo /etc/init.d/syslog stop

I had authentiation and most other options disabled on RP-PPPoE as I was just trying to get the server and client connected, and lo-and-behold, the client connected right away! This was really weird, since what does syslog have to do with RP-PPPoE and why would it stop the connection from working?

I did a bunch of searching, and found the following two posts:

[pppd and rp-pppoe LCP timeout issues](http://marc.info/?l=linux-ppp&m=118661751530643)

[Bug 222295 - ppp-2.4.3-6.2.1 conflict with syslog on pppoe-server.](https://bugzilla.redhat.com/show_bug.cgi?id=222295)

If you look through them, you'll see it has to do with pppd version 2.4.4, which is the version running (as can be seen in the /var/log/messages log above). You can also test which version of pppd you're using with `pppd --version`.

The specific issue has to do with the placement of a `closelog()` function call in main.c. Here's the patch that's included in the second post:

	--- ppp-2.4.4/pppd/main.c.orig 2006-06-04 07:52:50.000000000 +0400
	+++ ppp-2.4.4/pppd/main.c 2007-11-09 14:47:20.000000000 +0300
	@@ -1567,6 +1567,8 @@
	if (errfd == 0 || errfd == 1)
	errfd = dup(errfd);

	+ closelog();
	+
	/* dup the in, out, err fds to 0, 1, 2 */
	if (infd != 0)
	dup2(infd, 0);
	@@ -1575,7 +1577,6 @@
	if (errfd != 2)
	dup2(errfd, 2);

	- closelog();
	if (log_to_fd > 2)
	close(log_to_fd);
	if (the_channel->close)

As you can see, the patch just relocates the function call a few lines up. What I decided to do was download the latest version of pppd, ensure the patch was already applied, and install it overtop of pppd 2.4.4 already on the system. I downloaded the latest version (2.4.5) from [here](ftp://ftp.samba.org/pub/ppp/), inspected main.c to make sure the patch was already applied (which it was), and followed the installation instructions to install it.

Once I had 2.4.5 installed, I confirmed with `pppd --version`, ran the server and tried connecting with a client. I ended up needing to rebuild 2.4.5 after changing the install path in the configure script because it wasn't installing to the same directory as 2.4.4 and RP-PPPoE was still using 2.4.4. 2.4.4 was located in /usr/sbin, while the configure script configured 2.4.5 to install to /usr/local/sbin by default. After changing the configure script to install to /usr/sbin, RP-PPPoE used the correct version and the client connected without issue! The logs in /var/log/messages now look like this:

	pppd[4075]: pppd 2.4.5 started by root, uid 0
	pppd[4075]: Using interface ppp0
	pppd[4075]: Connect: ppp0 <--> /dev/pts/1
	pppd[4075]: local  IP address 10.0.0.1
	pppd[4075]: remote IP address 10.67.15.1

After I got rid of that bug, I started turning features back on (such as authentication), and everything worked.

The two main tutorials I read to actually set up RP-PPPoE were:

[Setting-up Basic PPPoE Server in Linux (x86_64 Platform)](http://darmawan-salihun.blogspot.ca/2008/12/setting-up-basic-pppoe-server-in-linux.html)

[PPPoE Server – How To Do It Yourself](http://www.howtodoityourself.org/2010/12/01/pppoe-server-how-to-do-it-yourself.html)