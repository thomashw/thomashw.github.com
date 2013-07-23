---
layout: post
title: "Adjust screen size in Remote Desktop Connection for Mac"
date: 2013-07-23 10:42
comments: true
categories: 
- windows
- remote desktop connection
- mac
---
I was recently using Remote Desktop Connection for Mac to remotely work on a Windows PC. One thing that bothered me was I wasn't able to scale the size of the screen. It was stuck at 1024x768, which was far too small for me to comfortably work.

One way to adjust the size of the screen is to edit the .rdp file. To do this, in the RDC file menu when your session is active click Save As and save the .rdp file somewhere on your system. Then exit the active session and open the .rdp file in your favorite text editor (I used Sublime Text). Find the **DesktopHeight** and **DesktopWidth** labels. You can increase both but probably want to ensure they have the same ratio. The default was 1024x768 (a 4:3 ratio) so I increased them to 1440x1080 (still a 4:3 ratio).

Then all you do is save the .rdp file, open it, and the new session should be at the new resolution.