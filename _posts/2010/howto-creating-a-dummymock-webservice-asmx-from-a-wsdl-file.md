---
ID: 36
post_title: 'HOWTO: Creating a Dummy/Mock Webservice (ASMX) from a WSDL File'
post_name: >
  howto-creating-a-dummymock-webservice-asmx-from-a-wsdl-file
author: David
post_date: 2010-11-29 14:41:01
layout: post
link: >
  https://blog.davidchristiansen.com/2010/11/howto-creating-a-dummymock-webservice-asmx-from-a-wsdl-file/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>Sometimes it is helpful to mock a xml webservice for the purpose of testing against it without invoking the actual service. There are several reasons why this can be a good idea, such as when the service is not hosted by you and therefore not under your control or in situations where calls to the service incur some kind of cost.</p>  <p>In my case, I wanted to mock out a service so that the scope of my unit testing was just my integration. I also wanted control over the service response to emulate a range of scenarios.</p>  <p>I would typically have developed a mock implementation using WCF however using an ASMX seemed the simplest method at the time, and I like the (Keep It Simple Stupid) KISS methodology.</p>  <p>So, here are the steps.</p>  <h2>Step 1 – Create the ASMX file to host the service</h2>  <p>In my case I already have a Test web application from which I host several test service stubs and other things, so it is simple for me to Right Click &gt; New Item &gt; Web Service. </p>  <p>Change the filename to something meaningful, such as &lt;ServiceName&gt;Mock.asmx</p>  <h2>Step 2 – Generate a service stub from WSDL</h2>  <p>Open a console window, I typically use the console that comes with Visual Studio (Available from start menu) and navigate to the folder containing the ASMX file you generated.</p>  <p>Type the following;</p>    <pre>wsdl &lt;URL of WSDL file&gt; /language:CS /server<br></pre>



<p>The URL is the full URL of the service you are wishing to stub out. In particular, I ensure the URL is that of the WSDL file.</p>

<p>/language can be your language of choice, in my case CS for C#. </p>

<p>/server specifies that you are wishing to generate code that would host such a service</p>

<p>A full list of options can by obtained by simply typing <em>wsdl</em> from the command line.</p>

<h2>Step 3 – Modify your ASMX to ‘host’ the generated code.</h2>

<p>By default the code behind for your ASMX will derive from </p>



<pre>System.Web.Services.WebService<br></pre>



<p>You want to change the code behind to derive instead from the code you generated in step 2. To find out which class to derive from, search your generated code for the class decorated with the <em>System.Web.Services.WebServiceAttribute</em>.</p>

<p>Once you have done this, compile your project. If things are as they should be the build should fail because you have not implemented the expected methods of the service.</p>

<p>Implement the services, throwing a </p>

<pre> throw new NotImplementedException();</pre>

<pre><br></pre>

<p>for now if you like – we just want it to compile.</p>

<p>Once you have done this, compile your project and open your ASMX in Internet Explorer (or your browser of choice)</p>

<p>If all is well, you should see a web page, with the name of the service at the top in a blue panel.</p>

<h2>Step 4 – Customise your service logic (Optional)</h2>

<p>As i mentioned, I wanted to be able to manage the responses I received from my service. This is done by modifying my implementation of the service that I have just created in the previous steps. Simples.</p>