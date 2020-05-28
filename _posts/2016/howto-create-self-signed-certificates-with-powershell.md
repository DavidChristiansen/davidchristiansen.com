---
ID: 533
post_title: 'HowTo: Create Self-Signed Certificates with PowerShell'
post_name: >
  howto-create-self-signed-certificates-with-powershell
author: David
post_date: 2016-09-08 15:32:35
layout: post
link: >
  https://blog.davidchristiansen.com/2016/09/howto-create-self-signed-certificates-with-powershell/
published: true
tags:
  - certificate authority
  - powershell
  - self-signed
categories:
  - Development (General)
---
This is a short post about how to create Self-Signed certificates with the <span class="lang:ps decode:true  crayon-inline ">New-SelfSignedCertificate</span>  PowerShell module.  More specifically, this post will cover creating your own Root Certificate, exporting public and PFX certificates, creating certificates signed by your root certificate authority.  Historically you would do this using the old-trusty makecert.exe, but nowadays we can do it straight from powershell! (oh joy!)

<!--more-->
<h2>Create Root Certificate</h2>
Here's what we are going to do:
<ol>
 	<li>Create the certificate;</li>
 	<li>Define a password string;</li>
 	<li>Export the certificate in PFX format, and secure it with the password you identified;</li>
 	<li>Export the public certificate and save it as a <code>.cer</code> file.</li>
</ol>
So let's get going.  In your powershell console, type the following (Replacing the dnsname with something relevant to you)
<pre class="lang:ps decode:true ">New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "Lab Root Certificate Authority"</pre>
<span style="font-size: 14px;">this will result in something along the line of the following being displayed in the console</span>
<blockquote>Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint Subject
---------- -------
F81AFDC2A23B8629580374E64871941476E43F02 CN=DC Lab Root Authority</blockquote>
You want to take a note of the <strong>thumbprint</strong> guid displayed as you will need this later.  In the case above, the thumbprint is <span class="lang:c# decode:true  crayon-inline ">F81AFDC2A23B8629580374E64871941476E43F02</span>

You then want to set up a variable a string value as the password
<pre class="lang:ps decode:true">$pwd = ConvertTo-SecureString -String "Please-Use-A-Really-Strong-Password" -Force -AsPlainText</pre>
<span style="font-size: 14px;">Now, let's export the root certificate as a PFX file and use the password we just set up.</span>
<pre class="lang:ps decode:true ">Export-PfxCertificate -cert cert:\localMachine\my\F81AFDC2A23B8629580374E64871941476E43F02  -FilePath root-authority.pfx -Password $pwd

</pre>
<span style="font-size: 14px;">Great, now you have the PFX file saved, let's export the public key of your root authority (this is what you will install in "Trusted Root Authorities" for example).  </span><em style="font-size: 14px;">NB: As it's the only the public certificate we are exporting, you do not need to set a password</em><span style="font-size: 14px;">.</span>
<pre class="lang:ps decode:true">Export-Certificate -Cert cert:\localMachine\my\F81AFDC2A23B8629580374E64871941476E43F02 -FilePath root-authority.crt
</pre>
<span style="color: #000000; font-family: 'Roboto Slab', sans-serif; font-size: 30px; font-weight: 400; letter-spacing: -0.015em; line-height: 32px;">Create Certificates signed by our Root Certificate Authority</span>

Here's what we are going to do:
<ol>
 	<li>Load the root authority certificate into memory;</li>
 	<li>Create a certificate signed by the root authority;</li>
 	<li>Define a password string to secure the PFX file;</li>
 	<li>Export the new certificate as a PFX file;</li>
 	<li>Export the new certificate as a CRT file</li>
</ol>
<h3>Load the root authority certificate</h3>
Load the root authority certificate is as easy as pie.  Below I am using the root cert authority thumbprint discovered above
<pre class="lang:ps decode:true ">$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\F81AFDC2A23B8629580374E64871941476E43F02 )</pre>
<span style="color: #000000; font-family: 'Roboto Slab', sans-serif; font-size: 24px; font-weight: 400; letter-spacing: -0.015em; line-height: 26px;">Create a certificate signed by the root authority</span>

OK, so now we want to create a new certificate, but this time have the root certificate authority sign it.
<pre class="lang:ps decode:true ">New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "Gateway Certificate" -Signer $rootcert</pre>
<em><span style="font-size: 14px;">nb: I am not using a resolvable dnsname in my example, but you can! (such as www.example.com)</span></em>

This will result in the thumbprint of the new certificate being displayed in the console.  Again, record this thumbprint for use later.
<h3>Define a password string</h3>
<pre class="lang:ps decode:true ">$pwd2 = ConvertTo-SecureString -String "Please-Use-A-Really-Strong-Password" -Force -AsPlainText</pre>
<span style="color: #000000; font-family: 'Roboto Slab', sans-serif; font-size: 24px; font-weight: 400; letter-spacing: -0.015em; line-height: 26px;">Export the new certificate as a PFX file</span>
<pre class="lang:ps decode:true ">Export-PfxCertificate -cert cert:\localMachine\my\A01F36FC2A7EB9CD506BD37E201935D6EB58A592  -FilePath gateway-certificate.pfx -Password $pwd2</pre>
<span style="color: #000000; font-family: 'Roboto Slab', sans-serif; font-size: 24px; font-weight: 400; letter-spacing: -0.015em; line-height: 26px;">Export new certificate public key as a CRT file</span>

Use the following to export the public key of the certificate.
<pre class="lang:ps decode:true  ">Export-Certificate -Cert cert:\localMachine\my\A01F36FC2A7EB9CD506BD37E201935D6EB58A592 -FilePath gateway.crt</pre>
<span style="font-size: 14px;">All done !! No makecert.exe or mmc to be seen :)</span>

have fun.

DC

ps. For extra fun, here are some other examples
<h3>Extra: Create Certificates with Subject Alternative Names (SAN)</h3>
<code>New-SelfSignedCertificate -DnsName domain.example.com,anothersubdomain.example.com -CertStoreLocation cert:\LocalMachine\My</code>
<h3>Extra: Create Wildcard Certificates</h3>
<code>New-SelfSignedCertificate -dnsname *.example.com -certstorelocation cert:\localmachine\my</code>