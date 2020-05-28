---
ID: 59
post_title: 'PCI-DSS Assessment &ndash; Howto: Disable SSL2 and Weak Ciphers on IIS6'
post_name: >
  pci-dss-assessment-howto-disable-ssl2-and-weak-ciphers-on-iis6
author: David
post_date: 2009-03-24 19:24:17
layout: post
link: >
  https://blog.davidchristiansen.com/2009/03/pci-dss-assessment-howto-disable-ssl2-and-weak-ciphers-on-iis6/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>If you deal with Credit Cards on the Internet, then it is very likely that you will have to conform to the Payment Card Industry Data Security Standards (PCI-DSS). You can <a target="_blank" href="https://www.pcisecuritystandards.org/security_standards/pci_dss.shtml">get the standards specification</a>, the <a target="_blank" href="https://www.pcisecuritystandards.org/saq/index.shtml">self assessment questionnaire</a>, or find instructions on exactly what you need to do to conform on the <a target="_blank" href="https://www.pcisecuritystandards.org/">PCI-Security Standards Council website</a>.</p>
<p>Depending on the nature of your business, and indeed how much money you see, you may need to perform network vulnerability assessments every quarter. These assessments are performed against the servers hosting your ‘payment application’ ie. your web server.</p>
<p>You will find a complete list of all companies that are recognised by the <a target="_blank" href="network vulnerability assessment">PCI on their website here (PDF)</a> and most will offer some kind of free limited network assessment (such as 5 free scans for up to 3 IPs per device)</p>
<p>So, you’ve run your network vulnerability assessment on your IIS6 server. Great. But now you received a “NOT COMPLIANT” report as a result of failing the following two tests:</p>
<ul>
    <li>“Deprecated SSL Protocol Usage” </li>
    <li>“Weak Supported SSL Ciphers Suites” </li>
</ul>
<p>Let’s look at each one in turn and how to solve them.</p>
<h6>Deprecated SSL Protocol Usage - The remote service encrypts traffic using a protocol with known weaknesses</h6>
<p>The report may also give you the following information to put the fear of your god into you - “The remote service accepts connections encrypted using SSL 2.0, which reportedly suffers from several cryptographic flaws and has been deprecated for several years. An attacker may be able to exploit these issues to conduct man-in-the-middle attacks or decrypt communications between the affected service and clients.”</p>
<p>You can confirm the reports findings by typing your hostname or IP into this handy utility - <a href="http://www.serversniff.net/sslcheck.php">http://www.serversniff.net/sslcheck.php</a> which will generate a report such as the picture below</p>
<p><img title="image" style="BORDER-TOP-WIDTH: 0px; DISPLAY: inline; BORDER-LEFT-WIDTH: 0px; BORDER-BOTTOM-WIDTH: 0px; BORDER-RIGHT-WIDTH: 0px" height="362" alt="image" width="755" border="0" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/image_3-png.png"> </p>
<p>The report is showing you that both SSL2 and SSL3 are enabled, and all the ciphers currently exposed by the server.</p>
<p>OK- So we want to remove SSL2 from this list. Unfortunately the only fix I know for this is by modifying some keys in your registry. There is a <a target="_blank" href="http://support.microsoft.com/kb/187498">Microsoft Support article (187498) here</a>which describes the following registry modifications.</p>
<pre class="brush: js">Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\Multi-Protocol Unified Hello\Server]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\PCT 1.0\Server]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server]
"Enabled"=dword:ffffffff
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server]
"Enabled"=dword:ffffffff
</pre>
<p> </p>
<p>What is happening here is that you are explicitly turning off PCT 1.0, SSL 2.0 and explicitly turning on SSL 3.0, TLS 1.0.</p>
<p>Running the report again on <a href="http://www.serversniff.net/sslcheck.php">http://www.serversniff.net/sslcheck.php</a> should present results something like below:</p>
<p><img title="image" style="BORDER-TOP-WIDTH: 0px; DISPLAY: inline; BORDER-LEFT-WIDTH: 0px; BORDER-BOTTOM-WIDTH: 0px; BORDER-RIGHT-WIDTH: 0px" height="266" alt="image" width="759" border="0" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/image_6-png.png"> </p>
<p>Right, now lets get rid of those weak ciphers.</p>
<h6>Weak Supported SSL Ciphers Suites - The remote service supports the use of weak SSL ciphers</h6>
<p>Again, another hard hitting description may be given - “The remote host supports the use of SSL ciphers that offer either weak encryption or no encryption at all”</p>
<p>OK. Here’s registry fix number 2.</p>
<pre class="brush: js">Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\DES 56/56]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\NULL]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC2 128/128]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC2 40/128]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC2 56/128]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128]
"Enabled"=dword:ffffffff
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 64/128]
"Enabled"=dword:00000000
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\Triple DES 168/168]
"Enabled"=dword:ffffffff</pre>
<p> </p>
<p>This time we are explicity turning off the ciphers identified in the Network Vulnerability report, leaving only the strong ones switched on.</p>
<p>Running the <a href="http://www.serversniff.net/sslcheck.php">http://www.serversniff.net/sslcheck.php</a> report again shows a completely new picture:</p>
<p><img title="image" style="BORDER-TOP-WIDTH: 0px; DISPLAY: inline; BORDER-LEFT-WIDTH: 0px; BORDER-BOTTOM-WIDTH: 0px; BORDER-RIGHT-WIDTH: 0px" height="173" alt="image" width="742" border="0" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/cfimage_9-png.png"> </p>
<p><strong>NOTE:</strong> I do hope this post helps you out but please note that I do not accept any responsibility for your actions. This post details what worked for me, YMMV. Take care, Backup first, and don’t blame me if it goes pear shaped. As Microsoft Support says : </p>
<p><font size="1">Important This post contains steps that tell you how to modify the registry. However, serious problems might occur if you modify the registry incorrectly. Therefore, make sure that you follow these steps carefully. For added protection, back up the registry before you modify it. Then, you can restore the registry if a problem occurs. For more information about how to back up and restore the registry, click the following article number to view the article in the Microsoft Knowledge Base: </font></p>
<p><a href="http://support.microsoft.com/kb/322756/"><font size="1">322756</font></a><font size="1"> How to back up and restore the registry in Windows</font></p>