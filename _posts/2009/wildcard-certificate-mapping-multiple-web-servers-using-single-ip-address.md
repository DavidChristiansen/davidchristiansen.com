---
ID: 45
post_title: >
  Wildcard Certificate Mapping Multiple
  Web Servers using Single IP address
post_name: >
  wildcard-certificate-mapping-multiple-web-servers-using-single-ip-address
author: David
post_date: 2009-06-01 12:13:05
layout: post
link: >
  https://blog.davidchristiansen.com/2009/06/wildcard-certificate-mapping-multiple-web-servers-using-single-ip-address/
published: true
tags: [ ]
categories:
  - Development (General)
---
<h2>Prerequisites</h2> <ul> <li>MakeCert.exe (Which should be part of a visual studio install or <a href="http://download.microsoft.com/download/platformsdk/Update/5.131.3617.0/NT45XP/EN-US/makecert.exe">downloadable here</a>)  </li><li>winhttpcertcfg.exe (<a href="http://www.microsoft.com/downloads/details.aspx?familyid=c42e27ac-3409-40e9-8667-c748e422833f&amp;displaylang=en">downloadable here</a>)  </li><li>APPCMD (Part of Vista / Server 2008 / Windows 7)  </li><li>'Certificates' snap-in for Personal and Local Computer using MMC</li></ul> <h2><strong>Method</strong></h2> <p>Execute the following command from a command prompt</p><pre><code>makecert -r -pe -n CN=*.domain.com -ss my -sr currentuser -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12 wildcard.domain.cer<br></code></pre>
<p>then 
</p><ul>
<li>From the RUN command or start menu, type MMC 
</li><li>File &gt; Add or Remove Snap-ins - Select Certificates, Click Add, Select My user account, Click Finish 
</li><li>Repeat previous step and select My Computer (then selecting Local Computer) 
</li><li>Click OK 
</li><li>Expand Certificates - Current User &gt; Personal &gt; Certificates 
</li><li>Right click *.domain.com and All Tasks &gt; Export. The PFX file contains both the public and private key for this cert, hence why your asked for a password. 
</li><li>Copy or Move the Certificate from Current User &gt; Personal &gt; Certificates to Local Computer &gt; Trusted Root Certification Authorities &gt; Certificates 
</li><li>Import the PFX into Local Computer &gt; Personal &gt; Certificates (this will be the certificate used by your web services.</li></ul>
<p>Now let's create your web servers 
</p><h4>Remove existing demo app pools and sites</h4><pre><code>%windir%\system32\inetsrv\Appcmd delete site "Demo 1"<br>%windir%\system32\inetsrv\Appcmd delete site "Demo 2"<br>%windir%\system32\inetsrv\Appcmd delete AppPool "Demo 1 App Pool"<br>%windir%\system32\inetsrv\Appcmd delete AppPool "Demo 2 Portal App Pool"</code></pre>
<h4>Establish SSL Environment</h4>
<p>Tell windows that Network Service is allowed access to your wildcard cert. and tell it to bind the cert to port 443 on your IP address</p><pre><code>PathToWinHTTPCertCfg\winhttpcertcfg -g -i "wildcard.domain.com.pfx" -c LOCAL_MACHINE\My -a “Network Service” -p MySecretPassword<br></code></pre><pre><code></code> </pre>
<p>Then execute the following </p><pre><code>netsh http add sslcert ipport=&lt;YOURLOCALIPADDRESS&gt;:443 certhash=&lt;CERTIFICATE THUMBPRINT&gt; appid=&lt;A GUID IN THE FORM OF {ab3c58f7-8316-42e3-bc6e-771d4ce4b201}&gt;</code></pre>
<h4>Create App Pools and Sites</h4>
<p>This is the code to create app pools and sites</p><pre><code>%windir%\system32\inetsrv\Appcmd add site -id:100 -name:"Demo 1" -bindings:http/*:80:YOURLOCALIPADDRESS -physicalPath:&lt;PathToDemo1Source&gt; -logfile.directory:&lt;PathToPutLogFilesIn&gt; -traceFailedRequestsLogging.directory:&lt;PathToPutTraceFiles&gt;</code></pre><pre><code></code> </pre><pre><code>%windir%\system32\inetsrv\Appcmd set app "Demo 1/" -applicationPool:"Demo 1 App Pool"<br></code></pre><pre><code>%windir%\system32\inetsrv\Appcmd set site /site.name:"Demo 1" /+bindings.[protocol='https',bindingInformation='*:443:demo1.domain.com']<br></code></pre><code><pre>%windir%\system32\inetsrv\Appcmd add site -id:200 -name:"Demo 2" -bindings:http/*:80:YOURLOCALIPADDRESS -physicalPath:&lt;PathToDemo2Source&gt; -logfile.directory:&lt;PathToPutLogFilesIn&gt; -traceFailedRequestsLogging.directory:&lt;PathToPutTraceFiles&gt;<br></pre><pre>%windir%\system32\inetsrv\Appcmd set app "Demo 2/" -applicationPool:"Demo 2 App Pool"<br></pre><pre>%windir%\system32\inetsrv\Appcmd set site /site.name:"Demo 2" /+bindings.[protocol='https',bindingInformation='*:443:demo2.domain.com']</pre></code><pre><code><br> </code></pre>
<p>...and that should be you ;) Enjoy! 
</p><ol></ol>