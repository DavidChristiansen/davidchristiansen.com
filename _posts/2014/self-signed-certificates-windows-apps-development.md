---
ID: 456
post_title: 'HowTo: Use self-signed certificates during Windows Apps development (no code required)'
post_name: >
  self-signed-certificates-windows-apps-development
author: David
post_date: 2014-12-16 23:14:15
layout: post
link: >
  https://blog.davidchristiansen.com/2014/12/self-signed-certificates-windows-apps-development/
published: true
tags:
  - certificate
  - howto
  - self-signed
  - windows phone
categories:
  - Development (General)
  - Security
---
&nbsp;
<blockquote>TL;DR; My solution was to save my self signed certificate in DER format, sticking it on a local IIS site, loading the URL to the certificate in my mobile emulator, opening the file (when prompted) and installing the certificate.  Having done this, my mobile now trusts that certificate and I am free to play to my hearts content over HTTPS.</blockquote>
<!--more-->
<img class="alignright" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb.png" alt="image" width="132" height="240" align="right" border="0" />I have recently been writing a sample application for implementing OpenID Connect upon the windows runtime (Universal App for Windows and Windows Phone) and ran into issues when attempting to connect to a service running over HTTPS but using a self signed certificate.

As would be expected, the phone doesn’t know anything about the certificate and rightly doesn’t proceed with the communication.

The annoying issue is when this happens without you receiving any notification – such as when I was using
<pre class="">WebAuthenticationBroker.AuthenticateAndContinue</pre>
Calls using the WebAuthenticationBroker when the certificate trust fails, will result in a misleading WebAuthenticationStatus being returned - namely <em><strong>UserCancel</strong></em>.

So a quick search found many ways to solve this issue through code or modifying my application manifest.  My issue with these solutions where the fact that, in my case, the error only exists in the development environment.  I certainly don’t want to write code into my client to ‘ignore certificate errors’.  I also don’t want to add all test certificates into the application manifest.
<h3>My Solution</h3>
Here's what I found worked for me.
<h4>1. Save self-signed certificate</h4>
The service I was hosting over HTTPS happened to be running under IIS Express.  IIS Express installs a self signed certificated on your machine so what we want to do is export that certificate into a file that can be downloaded by our phone. There are a couple of ways you can export the certificate, but the easiest way for now is to navigate to the service being hosted over HTTPS, clicking on the ‘secure padlock’ symbol, and selecting <strong>Certificate Information </strong>(Chrome)

<a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image1.png"><img style="display: inline; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb1.png" alt="image" width="242" height="214" border="0" /></a>

This presents the standard windows dialog for certificate information.  What you want to do is switch over to the details tab and click the <strong>Copy to file…</strong> button.  Follow through the rest of the <strong>Certificate Export Wizard</strong> steps and you will have a file containing the certificate.

<a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image2.png"><img style="display: inline; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb2.png" alt="image" width="194" height="240" border="0" /></a> <a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image3.png"><img style="display: inline; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb3.png" alt="image" width="194" height="240" border="0" /></a> <a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image4.png"><img style="display: inline; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb4.png" alt="image" width="240" height="237" border="0" /></a>
<h4>2. Host the certificate file on a web server</h4>
Now you just need to get that certificate onto your windows phone emulator somehow.  The way I did it was to simply place the cert file in the root of a local IIS website.  One thing you must do is add a mime-type to the IIS configuration to allow .cer files to be downloaded as I found that IIS doesn’t know how to deal with that file type by default.

<a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image5.png"><img style="display: inline; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb5.png" alt="image" width="242" height="205" border="0" /></a>

Simply navigate to your web site in IIS Manager, Switch to the MIME Types setting panel and add an entry for the <strong>.cer </strong>file extension with the mime-type of <strong>application/octet-stream.</strong>
<h4>3. Open the certificate in your emulator's browser and install</h4>
In a browser on your phone emulator, Navigate to the site hosting your certificate file, select <strong>OPEN</strong> when prompted, followed by <strong>Install. </strong>This should be all you need to do.

<a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image6.png"><img style="display: inline; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb6.png" alt="image" width="132" height="240" border="0" /></a> <a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image7.png"><img style="display: inline; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb7.png" alt="image" width="131" height="240" border="0" /></a> <a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image8.png"><img style="display: inline; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2014/12/image_thumb8.png" alt="image" width="135" height="244" border="0" /></a>
<h4>4. Test HTTPS connection</h4>
In a browser on your phone emulator, Navigate to your HTTPS site now and ensure that you no longer get certificate error warnings.