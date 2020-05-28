---
ID: 10
post_title: 'Solved: Chrome v18, self signed certs and &ldquo;signed using a weak signature algorithm&rdquo;'
post_name: >
  solved-chrome-v18-self-signed-certs-and-signed-using-a-weak-signature-algorithm
author: David
post_date: 2012-04-02 15:46:09
layout: post
link: >
  https://blog.davidchristiansen.com/2012/04/solved-chrome-v18-self-signed-certs-and-signed-using-a-weak-signature-algorithm/
published: true
tags: [ ]
categories:
  - Development (General)
---
So chrome has just updated itself automatically and you are now running v18 – great. Or is it…

If like me, you are someone that are running sites using a self-signed SSL Certificate (i.e. when running a site on a developer machine) you may come across the following lovely message;

<a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/chromeissue2.png" rel="lightbox"><img style="display: inline;" title="Y U NO LOVE MY CERT" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/chromeissue2_thumb.png" alt="WAT? Try explaining what a weak signature algorithm means to a non-tech!" width="640" height="238" /></a>

Fear not, this is likely as a result of you following instructions you found on the <a href="http://httpd.apache.org/docs/2.0/ssl/ssl_faq.html#selfcert" target="_blank">apache openssl site</a> which results in a self signed cert using the MD5 signature hashing algorithm.
<h4>Using OpenSSL</h4>
The simple fix is to generate a new certificate specifying to use the SHA512 signature hashing algorithm, like so;
<pre class="lang:batch decode:true">openssl req -new -x509 -sha512 -nodes -out server.crt -keyout server.key</pre>
Simples!

Now, you should be able to confirm the signature algorithm used is sha512 by looking at the details tab of certificate

<a href="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/chromeissue1.png" rel="lightbox"><img style="display: inline;" title="Confirming the signature algorithm" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/chromeissue1_thumb.png" alt="Confirming the signature algorithm" width="421" height="195" /></a>
<h4>Notes</h4>
<ul>
	<li>If you change your certificate, be sure to reapply any private key permissions you require – such as allowing access to the application pool user.</li>
</ul>