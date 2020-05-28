---
ID: 495
post_title: 'HowTo: Clear Nuget cache on a build server'
post_name: clear-nuget-cache-on-build-server
author: David
post_date: 2015-05-29 10:25:49
layout: post
link: >
  https://blog.davidchristiansen.com/2015/05/clear-nuget-cache-on-build-server/
published: true
tags:
  - cache
  - nuget
categories:
  - Development (General)
---
<div style="float: right;">
<p style="text-align: right;"><img class="alignnone wp-image-499 size-full" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2015/05/logo-1.png" alt="nuget cache clear" width="120" height="35" />
<img class=" wp-image-500 aligncenter" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2015/05/teamcity512-300x300.png" alt="teamcity512" width="48" height="48" /></p>

</div>
Do you need to clear the Nuget cache on a build server which runs as a windows service?
Simply navigate to the nuget cache store which is located in the temp directory for the system service accounts.
<pre class="">C:\Windows\System32\config\systemprofile\AppData\Local\NuGet\Cache
C:\Windows\SysWOW64\config\systemprofile\AppData\Local\NuGet\Cache
</pre>
Alternatively, you could tell nuget to bypass the nuget cache altogether when it installs packages by using the -nocache option in your install statement
<pre>nuget install ninject -nocache 
</pre>