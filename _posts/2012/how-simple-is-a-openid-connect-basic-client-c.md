---
ID: 4
post_title: 'How simple is a OpenID Connect Basic client? (C#)'
post_name: >
  how-simple-is-a-openid-connect-basic-client-c
author: David
post_date: 2012-07-16 00:48:03
layout: post
link: >
  https://blog.davidchristiansen.com/2012/07/how-simple-is-a-openid-connect-basic-client-c/
published: true
tags:
  - openid connect
categories:
  - OAuth
  - Security
---
John Bradley has just posted a great entry demonstrating how simple life is going to be for a Relying Party when it comes to OpenID Connect. I highly recommend <a href="http://www.thread-safe.com/2012/07/how-simple-is-openid-connect-basic.html" target="_blank">you go and read it</a>.
<blockquote>OpenID Connect provides a lot of advanced facilities to fulfill many additional features requested by the member community. It is full of features that go beyond basic Authentication. However, that does not mean that it cannot be used for the simple case for “Just Authentication”.</blockquote>
The sample code in John’s post is in PHP so I thought I would quickly provide the same samples in C#. here we go.
<h4>Making an OpenID Connect request</h4>
In order for the client to make an OpenID Connect request, it needs to have the following information about the server:
<ul>
	<li>client identifier - An unique identifier issued to the client (RP) to identify itself to the authorization server. (e.g. 3214244)</li>
	<li>client secret - A shared secret established between the authorization server and client used for signing requests.</li>
	<li>end-user authorization endpoint - The authorization server’s HTTP endpoint capable of authenticating the end-user and obtaining authorization. (e.g., https://server.example.com/authorize )</li>
	<li>token endpoint - The authorization server’s HTTP endpoint capable of issuing access tokens.</li>
</ul>
In the simplest cases, this information is obtained by the client developer, having read the server’s documentation and pre-registered their application. Then, for a bear bone authentication request you would put a link like this in the HTML page:
<pre class="lang:xhtml decode:true brush: html">&lt;a href="https://server.example.com/authorize?grant_type=code&amp;scope=openid&amp;client_id=3214244&amp;state=af1Ef"&gt;Login with Example.com&lt;/a&gt;</pre>
The user initiates login by clicking on the “Login with Example.com” link, and is taken to the server where she is asked username/password etc. if she is not logged into example.com yet. Once she agrees to login to  the RP, the browser is redirected back to the call back URL at the RP by 302 redirect. The PHP Server side code may look like:
<pre class="lang:c# decode:true brush: csharp;">Response.Redirect("https://client.example.com/cb?code=8rFowidZfjt&amp;state=af1Ef",true);</pre>
<em>Note: state is the parameter that is used to protect against XSRF.  It binds the request to the browser session.  It is recommended but not required in OAuth and has been omitted to make the example static.</em> That should be simple enough?
<h4>Calling the Token endpoint to get id_token</h4>
Now that the RP has the ‘code’, you need to get the id_token from the token endpoint. The id_token is the user login information assertion.  What do you do? Just GET it with HTTP Basic Auth using client_id, client_secret, and the code you got in the first step. Using C#, it would look like:
<pre class="lang:c# decode:true brush: csharp;">var code = Request.Form[”code”];
var client = new WebClient();
NetworkCredential credentials = new NetworkCredential("testuser", "testpass");
client.Credentials = credentials;
client.Headers.Add("Content-Type","application/json; charset=utf-8");
var responseJson = client.DownloadString(new Uri("https://server.example.com/token?code="+code));</pre>
The result, <strong>responseJson</strong>, will contain a JSON like this (line wraps for display purposes only):
<pre class="lang:js decode:true brush: text;">{ 
"access_token": "SlAV32hkKG",
"token_type": "Bearer",
"refresh_token": "8xLOxBtZp8",
"expires_in": 3600,
"id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.
eyJpc3MiOiJodHRwczovL3NlcnZlci5leGFtcGxlLmNvbSIsInVzZXJfaWQiOiIyND.
gyODk3NjEwMDEiLCJhdWQiOiJodHRwOi8vY2iwiZXhwIjoxxpZW50LmV4YW1wbGUuY29tIMzExMjgxOTcwfSA.
eDesUD0vzDH3T1G3liaTNOrfaeWYjuRCEPNXVtaazNQ”
}</pre>
For simple authentication we will ignore "access_token", "token_type" etc. What you only care about is the "id_token". "id_token" is encoded in a format called  <a href="http://tools.ietf.org/html/draft-jones-json-web-token-07">JSON Web Token (JWT)</a>. JWT is the concatenation of “header”, “body”, “signature” by periods (.). Since you are getting this directly through TLS protected channel that is verifying the identity of the server certificate, you do not need to check the signature for integrity, so you just take out the second portion of it and base64url_decode it to get the information out of the id_token. So in c# you may do something like:
<pre class="lang:c# decode:true brush: csharp;">JObject o = JObject.Parse(response);
var token = o.id_token;
var id_array = token.split('.');
var id_body = Convert.FromBase64String(id_array[1]);</pre>
The resulting assertion, id_body in the above example,  about the user (after pretty formatting)  is:
<pre class="lang:js decode:true brush: text">{
"iss": https://server.example.com,
"user_id": "248289761001",
"aud": "3214244",
"iat": 1311195570,
"exp": 1311281970
}</pre>
<strong>“iss”</strong> is showing the issuer of this token, in this case, the server.example.com. The issuer must match the expected issuer for the token endpoint, if it is different you must reject the token. the 'iss" is the name space of the  user_id, which is unique within the issuer and never reassigned. When the client stores the user identifier, it MUST store the tuple of the  user_id and iss. <strong>“aud”</strong> stands for “audience” and it shows who is the audience of this token. In this case, it is the RP's client_id. If it is different, you must reject the token. <strong>"iat"</strong> stands for the time the token was issued.  This can be ignored in this flow as the client is talking directly to the token endpoint. <strong>“exp”</strong> is the expiry time of the token. If the current time is after “exp”, e.g., in PHP, if  $exp &lt; time();  the RP should reject the token as well. So, that is it. Now you know who is the user, i.e., you have authenticated the user. All of the above in the form of code would be:
<pre class="lang:c# decode:true brush: csharp; ">private bool checkID(idBody, issuer, clientID){
    JObject o = JObject.Parse(idBody);
    if (o.iss != issuer)
        return false;
    if (o.aud != clientID)
        return false;
    if (o.exp &lt; DateTime.UtcNow)
        return false;
    return true;
}</pre>
--- Once again, be sure to go read John’s post (<a href="http://www.thread-safe.com/2012/07/how-simple-is-openid-connect-basic.html" target="_blank">http://www.thread-safe.com/2012/07/how-simple-is-openid-connect-basic.html</a>)