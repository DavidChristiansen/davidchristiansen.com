---
ID: 446
post_title: 'HowTo: Auto synchronise files upon network connection (Windows)'
post_name: >
  howto-auto-synchronise-files-upon-network-connection
author: David
post_date: 2014-08-18 14:26:10
layout: post
link: >
  https://blog.davidchristiansen.com/2014/08/howto-auto-synchronise-files-upon-network-connection/
published: true
tags: [ ]
categories:
  - Development (General)
  - Productivity
---
<img class="alignnone" style="display: inline; margin-bottom: 10px; float: right; margin-left: 10px;" title="Man 'accidently' throwing laptop" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/08/TestLab.grid-6x2.jpg" alt="TestLab.grid-6x2" width="251" height="168" align="right" />There are times where we need to work away from the office network and the safety of backup infrastructure.  What would be nice is to automatically synchronise files on my laptop whenever I connect to the office network.

Working locally on laptops, it should go without saying, carries a danger of loosing data either through “Doh!” moments or through hardware failure (travelling can indeed be hard on the little grey rectangular friends of ours) so we need a solution that provides some safety to our data – preferably without us having to remember to do anything.  Holding back the urge to write something bespoke for once I looked at what I already have my dispersal -  GIT, RoboCopy and Task Scheduler to the rescue - Let's start auto synchronising our files upon connecting to a given network

<!--more-->
<h2>Bring me synchronisation solutions</h2>
So I want a solution that gives me the security of having my data in a location other than my laptop – preferably with that location having backups of it’s own.  Carrying and using USB keys for backups are not an option – keys get lost and do not provide the file security I require.  I needed a solution that synchronises data on my laptop with an file store on the office network .  More over, i want this to happen automatically – I want something to detect when I am on the office network (either physically attached or over VPN).
<h2>Enter stage left : Task Scheduler</h2>
Turns out you can create a task (in task scheduler) that gets triggered when a specific event is raised by windows. There are a whole host of events to which you can attach but the one I am interested in lives under Microsoft-Windows-NetworkProfile/Operational &gt; Network Profile with the Event ID of 10000.  So let’s create a simple task that gets triggered by this event. To test this event out we will just launch a command prompt window.
<ol>
	<li>Open Task Scheduler (open Start Menu, Type <strong>Task</strong>)</li>
	<li>Right click on the <em>Task Scheduler Library </em>and select <strong>Create Basic Task
</strong><img style="display: inline;" title="1" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/08/1.png" alt="1" width="229" height="240" /></li>
	<li>Create a Basic Task that
<ol>
	<li>Specify a name for your task (click <strong>Next</strong>)</li>
	<li>Specify the trigger for your task as “When an specific Event is Logged” (click <strong>Next</strong>)</li>
	<li>Select <strong>Microsoft-Windows-NetworkProfile/Operational</strong> from the Log menu</li>
	<li>Select <strong>Network Profile </strong>from the source</li>
	<li>Type <strong>10000 </strong>in the event ID field (click <strong>Next</strong>)</li>
	<li>Select “Start a program” from the Action options list</li>
	<li>Type <strong>cmd</strong> into the “Program/Script” field (click <strong>Next</strong></li>
	<li>Check the “Open the Properties dialog …” checkbox and click <strong><strong>Finish</strong></strong><a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/08/CropperCapture6.gif"><img style="display: inline;" title="CropperCapture[6]" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/08/CropperCapture6_thumb.gif" alt="CropperCapture[6]" width="640" height="365" /></a></li>
</ol>
</li>
	<li>Switch to the <strong>Conditions </strong>tab and select from the network list the network which, upon connecting, you want the application to run.
<img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border: 0px;" title="3" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/08/3.png" alt="3" width="540" height="413" border="0" /></li>
	<li>That’s it !</li>
</ol>
&nbsp;

Disconnect from the network in question then reconnect – if you have configured everything correctly a command prompt window should launch automatically.

&nbsp;
<h2>Synchronise with RoboCopy</h2>
Robocopy offers us the easiest way to synchronise two folders.  We can change the task we created earlier to invoke the following command
<pre class="lang:batch decode:true">robocopy \\SourceServer\Share \\DestinationServer\Share /MIR /ZB /XA:H /W:5</pre>
<span style="font-size: 14px;"> </span>Here is an explaination of the options I chose:
<ul>
	<li>/MIR specifies that robocopy should mirror the source directory and the destination directory. Beware that this may delete files at the destination.</li>
	<li>/ZB ensures robocopy can resume the transfer of a large file in mid-file instead of restarting.  If that fails, try  using the backup method instead</li>
	<li>/XA:H makes robocopy ignore hidden files, usually these will be system files that we’re not interested in.</li>
	<li>/W:5 reduces the wait time between failures to 5 seconds instead of the 30 second default.</li>
</ul>
<h2>Synchronise with GIT</h2>
I want to bring some change mangement / version control to the files I work upon on my laptop – the obvious answer was to use a distributed version control system such as GIT.
<h3>Using GIT for version control</h3>
<ol>
	<li>Initialise a ‘bare’ repository within your office network share folder (in my case H:\Projects).  It is easiest in the long run if the folder is empty to begin with.</li>
	<li>Clone your office network share repository to your local drive (in my case C:\_Work)</li>
	<li>Commit any files you have locally in your working directory</li>
</ol>
Now we have a cloned repository we can commit changes locally as often as we desire, view file change history and even restore to any saved version we like.  But those changes are only local – in order to get to the desired state of having a version existing on the office network share we need to push all changes to the server.  We will modify our scheduled task to execute a “GIT PUSH” whenever we connect to the network.

Create a BAT file with contents along the lines of
<pre class="lang:batch decode:true ">@echo off
c:
cd \_Work
git add -A &amp;&amp; git commit -am "AutoSync commit"
git push</pre>
Change the scheduled task you created earlier to execute this BAT file.  Now every time you connect to the network the task will execute a GIT PUSH command that synchronises all your changes and change history to the server.  Bosh!