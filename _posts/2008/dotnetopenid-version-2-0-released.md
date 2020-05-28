---
ID: 84
post_title: DotNetOpenId version 2.0 released
post_name: dotnetopenid-version-2-0-released
author: David
post_date: 2008-04-12 15:44:49
layout: post
link: >
  https://blog.davidchristiansen.com/2008/04/dotnetopenid-version-2-0-released/
published: true
tags: [ ]
categories:
  - Development (General)
  - Security
---
<p>The open source OpenID C# library <a href="http://dotnetopenid.googlecode.com/">DotNetOpenID</a> has been released.  This is a really exciting release that adds full support for OpenID 2.0 while preserving full backward compatibility for interoperating with OpenID 1.x.  It is a mature library with lots of helps for diagnostics and debugging, and a balance between simplicity and extensibility.  For a complete list of enhancements from the last release, check out the <a href="http://code.google.com/p/dotnetopenid/wiki/VersionChanges">VersionChanges</a> page.</p>  <p>Here are the highlights of this library and particularly this release:</p>  <ul>   <li>Support for <strong>OpenID 2.0 Relying Parties and Providers</strong>, including but not limited to these features       <ul>       <li>Xri and i-name support </li>        <li>Directed identity support </li>        <li>More secure hashing algorithms (SHA-256) </li>        <li>Interop with Yahoo and other OpenID 2.0-only providers </li>        <li>Better security against replay attacks. </li>        <li>Send unsolicited positive assertions from providers to automatically log your users in to relying party web sites. </li>     </ul>   </li>    <li>Much more comprehensive testing of common scenarios and possible security exploits. </li>    <li>More comprehensive HTML-based identity discovery. </li>    <li><i>Completely</i> stateless mode support for Relying Parties (not even HttpApplication state). </li>    <li>New OpenIdMobileTextBox ASP.NET control. </li>    <li>All relying party ASP.NET controls now support immediate mode. </li>    <li>Improved support for custom stores that have to serialize associations (for databases, etc.) </li>    <li>Debugger attributes to make stepping through the code easier. </li> </ul>  <p>We've had over 150 beta testers leading to this release and fixed several bugs along the way.  Thanks to all those who helped field test this release!</p>  <p>This is a release that Project Members have worked very hard to build and write tests to make sure its as secure as possible.  Please consider supporting past and future development with a donation (any size).</p>  <p><a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=andrewarnott%40gmail.com&amp;item_name=JMPInline%20blog%20and%20open%20source%20projects&amp;no_shipping=1&amp;no_note=1&amp;tax=0&amp;currency_code=USD&amp;bn=PP-DonationsBF&amp;charset=UTF-8"><img title="Help me help you." alt="Make a donation with PayPal" src="https://www.paypal.com/en_US/i/btn/x-click-but04.gif" border="0"></a></p>