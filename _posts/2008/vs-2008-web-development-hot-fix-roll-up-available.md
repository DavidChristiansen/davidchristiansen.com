---
ID: 128
post_title: >
  VS 2008 Web Development Hot-Fix Roll-Up
  Available
post_name: >
  vs-2008-web-development-hot-fix-roll-up-available
author: David
post_date: 2008-02-11 15:41:34
layout: post
link: >
  https://blog.davidchristiansen.com/2008/02/vs-2008-web-development-hot-fix-roll-up-available/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>From <a href="http://weblogs.asp.net/scottgu/archive/2008/02/08/vs-2008-web-development-hot-fix-roll-up-available.aspx" target="_blank">Scott Guthrie</a></p>  <blockquote>   <p>One of the things we are trying to do with VS 2008 is to more frequently release public patches that roll-up bug-fixes of commonly reported problems.  Today we are shipping a hot-fix roll-up that addresses several issues that we've seen reported with VS 2008 and Visual Web Developer Express 2008 web scenarios.</p> </blockquote>  <p>You can download this hot-fix roll-up for free <a href="https://connect.microsoft.com/VisualStudio/Downloads/DownloadDetails.aspx?DownloadID=10826">here</a> (it is a 2.6MB download).  Below is a list of the issues it fixes:</p>  <p><strong><u>HTML Source view performance</u></strong></p>  <p><b></b></p>  <ul>   <li>Source editor freezes for a few seconds when typing in a page with a custom control that has more than two levels of sub-properties. </li>    <li>“View Code” right-click context menu command takes a long time to appear with web application projects. </li>    <li>Visual Studio has very slow behavior when opening large HTML documents. </li>    <li>Visual Studio has responsiveness issues when working with big HTML files with certain markup. </li>    <li>The Tab/Shift-Tab (Indent/Un-indent) operation is slow with large HTML selections. </li> </ul>  <p><b></b></p>  <p><b><u>Design view performance</u></b></p>  <p><b></b></p>  <ul>   <li>Slow typing in design view with certain page markup configurations. </li> </ul>  <p><b><u>HTML editing</u></b></p>  <ul>   <li>Quotes are not inserted after <i>Class</i> or <i>CssClass</i> attribute even when the option is enabled. </li>    <li>Visual Studio crashes when <i>ServiceReference</i> element points back to the current web page. </li> </ul>  <p><b><u>JavaScript editing</u></b></p>  <p><b></b></p>  <ul>   <li>When opening a JavaScript file, colorization of the client script is sometimes delayed several seconds. </li>    <li>JavaScript IntelliSense does not work if an empty string property is encountered before the current line of editing. </li>    <li>JavaScript IntelliSense does not work when jQuery is used. </li> </ul>  <p><b><u>Web Site build performance</u></b></p>  <p><b></b></p>  <ul>   <li>Build is very slow when Bin folder contains large number of assemblies and .refresh files with web-site projects. </li> </ul>