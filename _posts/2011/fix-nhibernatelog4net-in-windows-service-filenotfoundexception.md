---
ID: 32
post_title: 'FIX: nHibernate/log4net in windows service. FileNotFoundException'
post_name: >
  fix-nhibernatelog4net-in-windows-service-filenotfoundexception
author: David
post_date: 2011-01-20 14:51:44
layout: post
link: >
  https://blog.davidchristiansen.com/2011/01/fix-nhibernatelog4net-in-windows-service-filenotfoundexception/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>When using nHibernate in a windows service you may find that you encounter FileNotFoundExceptions when trying to load assemblies or external configuration files. Typically this is in scenarios where you are running your service under a user context such as NetworkService.</p>  <p>If you hunt around long enough you will find that it is trying to locate said files in the %WINDIR%\system32 folder (for example c:\windows\system32), and not from the services own directory.</p>  <p>By default window services set the default directory to the %WINDIR%\system32.</p>  <p>Simply change your service's default directory to wherever your service is running.</p>  <p>Here's a sample:</p>  <pre class="brush: csharp">// Set current directory to assembly folder
// Need to to this so can find the configuration file for the logger
// default is %WINDIR%\system32 for window services
static void Main(string[] args) {
	Environment.CurrentDirectory = System.IO.Path.GetDirectoryName(<br>	System.Reflection.Assembly.GetEntryAssembly().Location);
///...
}</pre>