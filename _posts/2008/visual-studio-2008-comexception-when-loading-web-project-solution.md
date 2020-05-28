---
ID: 126
post_title: 'Visual Studio 2008 &#8211; COMException when loading web project &#8211; Solution'
post_name: >
  visual-studio-2008-comexception-when-loading-web-project-solution
author: David
post_date: 2008-02-16 17:47:18
layout: post
link: >
  https://blog.davidchristiansen.com/2008/02/visual-studio-2008-comexception-when-loading-web-project-solution/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>I recently discovered the cause and resolution of an error (Microsoft Visual Studio - System.Runtime.InteropServices.COMException) I received when loading a web project in Visual Studio 2008.</p>  <p><img style="border-right: 0px; border-top: 0px; border-left: 0px; border-bottom: 0px" height="149" alt="System_Runtime_InteropServices_COMException" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2012/10/comexception.png" width="330" border="0"></p>  <p>Whilst it would appear that this message can be displayed for a number of reasons (see Bug Report at bottom of post), in my case it was through the web project not being accessible through IIS...</p>  <p> </p>  <p>Specifically, I had change the hostheader name of the IIS Virtual Host - the visual studio project had not been updated to point to the new hostheader. Simply close the solution, update the project file to point to the new url and load it again. bingo!</p>  <p> </p>  <p><strong>Associated Microsoft Bug Report</strong> - <a title="http://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=317124" href="http://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=317124">http://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=317124</a></p>  <p> </p>  <div class="wlWriterSmartContent" id="scid:0767317B-992E-4b12-91E0-4F059A8CECA8:37da7c39-46e9-463b-8061-642d393f1317" style="padding-right: 0px; display: inline; padding-left: 0px; padding-bottom: 0px; margin: 0px; padding-top: 0px">Technorati Tags: <a href="http://technorati.com/tags/Visual%20Studio" rel="tag">Visual Studio</a></div>