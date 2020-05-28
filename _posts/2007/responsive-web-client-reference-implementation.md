---
ID: 176
post_title: >
  Responsive Web Client Reference
  Implementation
post_name: >
  responsive-web-client-reference-implementation
author: David
post_date: 2007-10-29 15:45:29
layout: post
link: >
  https://blog.davidchristiansen.com/2007/10/responsive-web-client-reference-implementation/
published: true
tags: [ ]
categories:
  - Development (General)
---
<p>This week has seen the folks at Patterns and Practices shipping a <a href="https://www.codeplex.com/Release/ProjectReleases.aspx?ProjectName=websf&amp;ReleaseId=7630">Reference Implementation</a> for the web client bundle.  I can't wait to have time to look into this - nuggets like this can really make your development life so much easier. Surely anything that takes 10 to 15 degrees of your learning curve can only be a good thing, no?</p>  <blockquote>   <p><em>The Responsive Web Client Reference Implementation is a executable Order Entry sample application the demonstrates the Web client guidance in action. </em></p> </blockquote>  <p>Here is a brief summery of what the reference implementation shows (effortlessly pinched from <a href="http://blogs.msdn.com/brada/archive/2007/10/28/reference-implementation-of-the-web-client-bundles-from.aspx" target="_blank">Brad</a>, who himself seamlessly stole from Blaine)</p>  <ul>   <li><b>Composability</b> : Building a composite web application       <ul>       <li><b>Modularity</b>: Building complex sites based on modules that can be independently developed, tested, versioned, and deployed </li>        <li><b>Page composition</b>: Creating composite Web pages that contain multiple user control views. The user controls can be used across modules and can be independently developed, tested, versioned, and deployed. </li>        <li><strong>Testing of UI Logic</strong>: Utilizing unit tests to test Web page logic </li>     </ul>   </li>    <li><b>Responsiveness</b> : Improving usability and performance of the composite user interface utilizing ASP.NET AJAX.       <ul>       <li><b>Autocomplete</b> : Providing a list of suggestions to a user that are retrieved based on the characters they have entered. </li>        <li><b>Validation</b> : Validating user input on both the Web browser and the Web server.  </li>        <li><b>Live Form</b>: Dynamically updating a field in the browser based on input from another field. </li>        <li><b>Live Search</b> : Searching against LOB data from within the browser.  </li>        <li><b>Live Grid</b> : Adding, Removing and Modifying records in a grid. </li>        <li><b>Popup</b> : Utilizing AJAX popup's to dynamically display data that is retrieved from the server. </li>        <li><b>JSON Service</b>: Calling a Web service from the browser. </li>     </ul>   </li>    <li><b>UI Appearance</b>: Creating and modifying the look and feel of the UI       <ul>       <li><b>Layout management</b>: Creating a common user experience across different independent modules, separating the responsibility of the UI design from UI development. </li>        <li><b>User profile–based UIs</b>: Changing the behavior of the UI based on the user identity and profile information </li>        <li><b>Navigation</b>: Providing role-based site navigation. </li>     </ul>   </li>    <li><b>Security</b> : Improving site security       <ul>       <li><b>Authentication</b>: Identifying registered users of a site </li>        <li><b>Authorization</b>: Changing permissions for different users </li>        <li><b>Data security</b>: Validating input data to reduce the probability of cross-site scripting and SQL injection attacks.  </li>     </ul>   </li>    <li><b>Manageability</b>:       <ul>       <li><b>Easy deployment</b>: deploying and updating modules independently of other modules. </li>        <li><b>Logging and Exception Management</b>: Providing a standard logging and exception management approach to ensure the operations team can more easily operate the application. </li>     </ul>   </li> </ul>  <p>I encourage you to join me and <a href="https://www.codeplex.com/Release/ProjectReleases.aspx?ProjectName=websf&amp;ReleaseId=7630">take a look</a> and see if this work can help you in your next product.</p>