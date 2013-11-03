---
layout: post
title: "Killing an unresponsive SSH session"
date: 2013-11-03 11:35
comments: true
categories: 
- ssh
- terminal
---
Lately I’ve been testing an application I wrote that will perform firmware and configuration upgrades on a Cisco router and then reboot it once the upgrade is done to apply the upgrade. These are being done over an SSH session, so when the router reboots my SSH session becomes unresponsive.

Once this happens you can’t interact with the terminal session anymore because the SSH client catches all of the commands and since the SSH session is toast they don’t do anything. 'Ctrl + c' doesn’t exit the session because of this.

Previously I’d either just exit the terminal window and open a new one or wait until the connection came back. But recently I discovered there’s a command to exit the SSH session, which is `return` + `~` + `.`

That means you hit *return*, type tilda, and then type a period. This will exit the SSH session for you and let you continue working in the same terminal window.
