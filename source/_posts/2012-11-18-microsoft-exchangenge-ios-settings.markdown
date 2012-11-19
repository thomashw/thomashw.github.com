---
layout: post
title: "Microsoft Exchangenge iOS Settings"
date: 2012-11-18 17:36
comments: true
categories:
- exchange
- ios
- email
- microsoft
---
My company doesn’t officially support adding a Microsoft Exchange email/calendar account to your iOS device unless you install a certain application and certificate. I didn’t want to do this since it enables them to do a lot more than just enable email on my phone.
 
We have Microsoft Exchange webmail accessible from anywhere so I eventually figured out the correct settings to use from the webmail URL to add a Microsoft Exchange account to my iOS devices that pushes my work email, address book, and calendar.
 
Once I log in, our webmail URL looks something like this: https://subdomain.domain.com/owa
 
The settings I used to add the Microsoft Exchange account to my iOS devices were:
Server: subdomain.domain.com
Domain: domain
Username: same username as logging in to webmail (no domain required)
Password: same password
Use SSL: On
 
Once I used those settings and clicked Done checkmarks appeared beside each form field and then the account was added. It took maybe 5-10 minutes before I could access my email and calendar (I got a couple error messages during that time). I think it was while emails were being downloaded.
