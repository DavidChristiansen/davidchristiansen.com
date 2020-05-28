---
ID: 30
post_title: 'LogParser&ndash;Graphing PING results'
post_name: logparsergraphing-ping-results
author: David
post_date: 2011-01-25 09:33:32
layout: post
link: >
  https://blog.davidchristiansen.com/2011/01/logparsergraphing-ping-results/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>Here is a nifty little example of how to pipe console output to Microsoft <a title="Link to Microsoft Log Parser v2.2" target="_blank" href="http://www.microsoft.com/downloads/details.aspx?FamilyID=890cd06b-abf8-4c25-91b2-f8d975cf8c07&amp;displaylang=en">LogParser</a>, in this case the results of a ping against Google.</p>
<pre class="brush:csharp">
ping -n 15 www.google.co.uk | "%pathTologparser%\LogParser"
"SELECT TO_INT(REPLACE_STR(EXTRACT_VALUE (Text,'time',' '),'ms',''))
AS Response INTO Ping.gif FROM stdin WHERE Text LIKE '%%Reply%%'
GROUP BY Response" -i textline -legend off -chartTitle "Ping Times" -view</pre>
<p><strong>Note: </strong>I have inserted line breaks for readability, this should be written as one line.</p>
<p>Replace %pathTologparser% with the path to your local LogParser installation, typically c:\Program Files\Log Parser 2.2 or c:\Program Files (x86)\Log Parser 2.2 on x64 installations.</p>