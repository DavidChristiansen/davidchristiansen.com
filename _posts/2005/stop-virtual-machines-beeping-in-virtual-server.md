---
ID: 218
post_title: >
  Stop Virtual Machines BEEPING in Virtual
  Server
post_name: >
  stop-virtual-machines-beeping-in-virtual-server
author: David
post_date: 2005-10-06 07:59:00
layout: post
link: >
  https://blog.davidchristiansen.com/2005/10/stop-virtual-machines-beeping-in-virtual-server/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>This is a problem that has really annoyed me (and those in my office, and <a href="http://blogs.technet.com/jhoward/archive/2005/06/06/405915.aspx">others</a>) for a while. Basically, every darn message box or system alert causes the 1970's speaker in your PC to shreak out a hangover-unfriendly beep.</p>
<p>Goood news tho, it's a Simple fix!</p>
<p>Win2k3&nbsp;/ XP</p>
<ul>
<li>run <strong>sc config beep start= disabled </strong>from the command line</li></ul>
<p>There is also a knowledge base article which details a couple more fixes&nbsp;at <a href="http://support.microsoft.com/?id=838671">http://support.microsoft.com/?id=838671</a></p>