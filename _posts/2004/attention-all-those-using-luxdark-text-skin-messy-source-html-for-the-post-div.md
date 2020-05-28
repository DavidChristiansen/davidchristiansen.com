---
ID: 252
post_title: 'Attention all those using LuxDark .Text Skin &#8211; messy source HTML for the POST DIV'
post_name: >
  attention-all-those-using-luxdark-text-skin-messy-source-html-for-the-post-div
author: David
post_date: 2004-11-11 18:34:00
layout: post
link: >
  https://blog.davidchristiansen.com/2004/11/attention-all-those-using-luxdark-text-skin-messy-source-html-for-the-post-div/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>All those who are using the LUXDARK .Text theme (as I am) should note that the source code has a class attribute that can cause things to look fooie when a width attribute is added to the POST DIV.</p>
<p>Checking your source code (by viewing source) will prove it to you... Search for </p>
<div class="codePanel">DIV class=post style="FILTER: progid:DXImageTransform.Microsoft.Shadow(color='Blue', Direction=135, Strength=32)"&gt;</div><p>&nbsp;</p>
<p>I have added the following code into the <strong>News/Announcements </strong>part of .Text Admin for my blog which seems to do the job once the page has loaded.</p>

<div class="codePanel">window.onload = function(){<br>var fixPostDivs = document.getElementsByTagName("DIV")<br>for (var a = 0; a &lt; fixPostDivs.length; a++)<br>{ <br>if (fixPostDivs[a].className=='post')<br>{<br>fixPostDivs[a].style.filter= '';<br>}<br>}<br>};<br></div>