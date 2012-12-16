---
layout: post
title: "Newznab Release Issue"
date: 2012-12-16 10:46
comments: true
categories: 
- newznab
- usenet
---
When NZBMatrix was shut down I eventually came upon Newznab and installed it on my local server. After enabling a few groups I ran update_binaries.php and it seemed like it was working, but when I ran update_releases.php after update_binaries.php had finished it didn’t create any releases. The issue is the regex’s (or lack of) that come with the free version of Newznab. One easy fix for this is to donate to the project and get Newznab+. This includes the most up-to-date regex’s.

If you want to just try out Newznab and want to get some releases on your site before you donate, you can do the following (this is a Linux example):

    cd /tmp
    wget https://raw.github.com/kop1/newznab/master/db/latestregex.sql
    mysql -u USERNAME -p DATABASENAME < latestregex.sql

That will update your regex's so update_releases.php will create some releases. If you're happy with Newznab I highly suggest donating and getting Newznab+. It comes with lots of improvements and you're directly supporting the people who work on it.