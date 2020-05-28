---
ID: 22
post_title: 'DotNetOpenAuth: Debugging and Tracing OpenID and OAuth on ASP.NET (or MVC) using Glimpse'
post_name: >
  dotnetopenauth-debugging-and-tracing-openid-and-oauth-on-asp-net-or-mvc-using-glimpse
author: David
post_date: 2011-07-11 21:03:58
layout: post
link: >
  https://blog.davidchristiansen.com/2011/07/dotnetopenauth-debugging-and-tracing-openid-and-oauth-on-asp-net-or-mvc-using-glimpse/
published: true
tags: [ ]
categories:
  - DotNetOpenAuth
  - Security
---
<p><em>Synopsis:</em> Understanding exactly what is happening under the hood when it comes to working with OpenID and OAuth can be challenging even for the seasoned IDM developer. What I have found to help, is being able to see the communications between all the parties involved. Fortunately the DotNetOpenAuth library can be told to expose a plethora of information to the developer via integrated logging. In this post I will talk about a project called <strong><a href="http://getglimpse.com" target="_blank">Glimpse</a></strong> that exposes a whole host of information to you, the developer, directly within the browser and then I will introduce a Glimpse plugin I have written that exposes all that lovely juicy information directly from DotNetOpenAuth.</p>  <h3>In Short</h3>  <ol>   <li><strong><a href="http://nuget.org/List/Packages/DCCreative.DNOA4Glimpse" target="_blank">Get DNOA4Glimpse</a></strong>:       <br><font size="1">NuGet Command: PM&gt; <em>Install-Package DCCreative.DNOA4Glimpse</em></font> </li> </ol>  <h2>What is Glimpse?    <br><font size="1"><font style="font-weight: normal">(</font></font><a href="http://getglimpse.com/About"><font size="1"><font style="font-weight: normal">http://getglimpse.com/About</font></font></a><font size="1"><font style="font-weight: normal">)</font></font></h2>  <p><a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/whatisglimpse_5-jpg.jpg" rel="lightbox"><img style="background-image: none; border-right-width: 0px; margin: 0px 10px 0px 0px; padding-left: 0px; padding-right: 0px; display: inline; float: left; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="WhatIsGlimpse" border="0" alt="WhatIsGlimpse" align="left" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/whatisglimpse_thumb_1-jpg.jpg" width="142" height="240"></a>Glimpse is a very cool set of utilities that provide developers with a massive array of how requests go about being served, as well as a host of other information about the server itself.</p>  <blockquote>   <p>At its core, Glimpse allows you to debug your web site or web service right in the browser. Glimpse allows you to "Glimpse" into what's going on in your web server. In other words what Firebug is to debugging your client side code, Glimpse is to debugging your server within the client.</p> </blockquote>  <p>Glimpse is available via NuGet at <a href="http://nuget.org/List/Packages/Glimpse">http://nuget.org/List/Packages/Glimpse</a>.</p>  <div style="clear: both"></div>  <h2>Exposing DotNetOpenAuth to Glimpse</h2>  <p><a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/dnoa4glimpse.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto; padding-top: 0px" title="CropperCapture[2]" border="0" alt="CropperCapture[2]" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/dnoa4glimpse_thumb_1-png.png" width="240" height="187"></a></p>  <p>Writing a plugin for Glimpse is childsplay. Glimpse exposes a friendly Plugin interface</p>  <pre class="brush: csharp;">public interface IGlimpsePlugin
{
	string Name { get; }
	object GetData(HttpContextBase context);
	void SetupInit();
}</pre>

<p>Simply inherit from IGlimpsePlugin then implement the members in your plugin.</p>

<pre class="brush: csharp">[GlimpsePlugin]
public class DotNetOpenAuthPlugin : IGlimpsePlugin {
	public void SetupInit() {
	//...
	}
	public string Name {
		get { return "DotNetOpenAuth"; }
	}
}</pre>

<p>After adding a reference to the assembly containing your plugin, Glimpse will automatically pick up your plugin (thanks to the <a href="http://blogs.msdn.com/b/gblock/archive/tags/mef/" target="_blank">wonderful powers of MEF</a>).</p>

<h3>Demo</h3>

<p>I quickly threw together a sample application to test the plugin.</p>

<ol>
  <li>Create a new ASP.NET MVC site based on the DNOA MVC relying party sample.</li>

  <li>Add the DNOA4Glimpse package (<font size="1"><em>Install-Package DCCreative.DNOA4Glimpse)</em></font> </li>

  <li>Done (<a href="https://github.com/DavidChristiansen/DNOA4Glimpse/tree/master/source/DotNetOpenAuth.MVC3.Glimpse.TestStub" target="_blank">Get the source here</a>)</li>
</ol>

<p><a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/dnoa4glimpse_7-png.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto; padding-top: 0px" title="CropperCapture[2]" border="0" alt="CropperCapture[2]" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/dnoa4glimpse_thumb_2-png.png" width="617" height="480"></a></p>

<p>Glimpse has been turned on (by visiting <a href="http://yourwebsiteurl.example.com/glimpse.axd">//yourwebsiteurl.example.com/glimpse.axd</a>) which results in a panel being displayed at the bottom of your screen. As you can see, there is a DotNetOpenAuth tab. Awesome!</p>

<p>Right, now – let’s do an OpenID Authentication</p>

<p><a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/dnoa4glimpse_2-png.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto; padding-top: 0px" title="CropperCapture[4]" border="0" alt="CropperCapture[4]" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/dnoa4glimpse2_thumb-png.png" width="640" height="409"></a></p>

<p>What’s really cool is the Glimpse’s handling of complex objects.</p>

<p><a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/dnoa4glimpse3_2-png.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto; padding-top: 0px" title="CropperCapture[5]" border="0" alt="CropperCapture[5]" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/dnoa4glimpse3_thumb-png.png" width="642" height="343"></a></p>

<p>The presentation of complex objects such as the YADIS services detailed above will improve in a version I am currently working on, thanks for some new presentation features coming to Glimpse soon. Watch this space.</p>

<p>So the plugin is still in beta but hopefully you will find it useful.</p>

<p>DNOA4Glimpse and Demo Source is available at <a href="https://github.com/DavidChristiansen/DNOA4Glimpse">https://github.com/DavidChristiansen/DNOA4Glimpse</a></p>