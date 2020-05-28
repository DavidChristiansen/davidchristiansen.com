---
ID: 510
post_title: 'Fix: Blurred text in Office 2016 (Skype for Business 2016)'
post_name: >
  fix-blurred-text-in-office-2016-skype-for-business-2016
author: David
post_date: 2015-12-07 14:21:11
layout: post
link: >
  https://blog.davidchristiansen.com/2015/12/fix-blurred-text-in-office-2016-skype-for-business-2016/
published: true
tags:
  - fix
categories:
  - Development (General)
---
<img class="alignright" style="display: inline; float: right;" title="clip_image002" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2015/12/clip_image002_thumb.jpg" alt="Example of blurred text in Skype for Business UI" width="235" height="58" align="right" />

<strong>Just a heads up.</strong>  If after you install Office 2016 you discover that the Skype For Business UI displays with horrendous burred font and scaling issues, then I may have a fix for you.

The fix is rather simple…
<ol>
	<li>Switch to Outlook</li>
	<li>Go to File &gt; Options &gt; Advanced &gt; and Turn off hardware acceleration</li>
	<li>Restart Skype for Business 2016</li>
</ol>
…and your problem should be solved.

DC