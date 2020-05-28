---
ID: 34
post_title: '(Solved) ID6035: Cannot create a HashAlgorithm / &ldquo;Object Identifier (OID) is unknown&rdquo;'
post_name: >
  solved-id6035-cannot-create-a-hashalgorithm-object-identifier-oid-is-unknown
author: David
post_date: 2010-11-29 15:07:25
layout: post
link: >
  https://blog.davidchristiansen.com/2010/11/solved-id6035-cannot-create-a-hashalgorithm-object-identifier-oid-is-unknown/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>Implementing the <strong><a href="http://msdn.microsoft.com/en-us/security/aa570351.aspx">Windows Identity Foundation</a></strong> of Windows Server 2003 and experiencing errors such as the following?</p>  <p><em><strong>Exception:       <br></strong>System.NotSupportedException, mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</em></p>  <p><em><strong>Error Message:       <br></strong>ID6035: Cannot create a HashAlgorithm with name '</em><a href="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'"><em>http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</em></a><em>using the 'System.IdentityModel.Tokens.X509AsymmetricSecurityKey' crypto provider. SHA256 may require a minimum platform of Windows Server 2003 and .NET 3.5 SP1.</em></p>  <p>The same error can be experienced if you try to use FedUtil.exe on Windows 2003. Attempts to do so will result in the following error message;</p>  <p><em>Object Identifier (OID) is unknown</em></p>  <p>No doubt you have ensured that you are indeed on Windows Server 2003 (doh!) and indeed have .NET Framework v3.5 with Service Pack 1.</p>  <p>This error is due to, in some cases, the HashAlgorithm not being registered on Windows 2003.</p>  <h3>The Fix</h3>  <p>Microsoft have published a HotFix that solves this issue. I have found that the hotfix requires a server restart upon installation.</p>  <ul>   <li><a href="http://support.microsoft.com/hotfix/KBHotfix.aspx?kbnum=968730&amp;kbln=en-us">Microsoft HotFix Download</a></li> </ul>