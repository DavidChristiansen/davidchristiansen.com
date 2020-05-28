---
ID: 88
post_title: >
  DotNetOpenId 1.0 released, 2.0 beta also
  released
post_name: >
  dotnetopenid-1-0-released-2-0-beta-also-released
author: David
post_date: 2008-04-03 22:31:43
layout: post
link: >
  https://blog.davidchristiansen.com/2008/04/dotnetopenid-1-0-released-2-0-beta-also-released/
published: true
tags: [ ]
categories:
  - Development (General)
  - Security
---
<h2>DotNetOpenID 1.0 released!</h2>  <blockquote>   <p><a href="http://blog.nerdbank.net/" target="_blank"><em>Andrew Arnott</em></a></p>    <p>The culmination of a great deal of work in refactoring, bug fixing and enhancements can be found in the latest .NET implementation of the OpenId library known as <a href="http://code.google.com/p/dotnetopenid/downloads/list">DotNetOpenId v1.0</a>.  It is free and released under the <a href="http://www.opensource.org/licenses/bsd-license.php">New BSD license</a>, so give it a try!</p> </blockquote>  <p>Check the <strong><a href="http://code.google.com/p/dotnetopenid/wiki/VersionChanges" target="_blank">Version Changes page</a> </strong>for a list of important changes over the     <br>0.1.2 version.  Please note that there are a lot of breaking changes between this and 0.1.2, but on a plus side, there are no breaking changes between 1.0 and 2.0 (so far), so making the extra effort to adopt 1.0 today is recommended and only help pave the way for 2.0 when it is ready. </p>  <h2>DotNetOpenId 2.0 beta </h2>  <p>The <strong>DotNetOpenId 2.0 beta </strong>is also ready for field testing.  It is feature     <br>complete, offering a full implementation of the <a href="http://openid.net/specs/openid-authentication-2_0.html" target="_blank">OpenId 2.0 spec</a> except for     <br>extensions that are passed *without *any side-by-side authentication.  With the huge improvements brought by the OpenID 2.0 specification I am really pleased to see that this release become available.</p>  <p>With the one exception listed, these features are *already* in the source code: </p>  <p>   - Added support for *OpenID 2.0 Relying Parties and Providers*,    <br>   including but not limited to these features:     <br>      - Xri and i-name support     <br>      - Directed identity support     <br>      - More secure hashing algorithms (SHA-256)     <br>      - Interop with Yahoo and other OpenID 2.0-only providers     <br>      - Better security against replay attacks.     <br>   - *Planned:* Much more comprehensive testing of common scenarios and     <br>   possible security exploits.     <br>   - More comprehensive HTML-based identity discovery.     <br>   - *Completely* stateless mode supported for Relying Parties (not even     <br>   HttpApplication state). </p>