---
title:  Fiddler change response
#author: Jan Helberg
date:   2023-09-01 00:05:00 +0200
categories: [Software, Fiddler Classic]
tags: [Software, Fiddler, Fiddler Classic]
---

## What is a Fiddler?
Fiddler is a widely used web debugging proxy tool. It's primarily used by developers, testers, and network administrators to monitor, intercept, and analyse the HTTP/HTTPS traffic between a client (such as a web browser or a mobile app) and a server. You also have the capability to alter requests and responses prior to their receipt by a system (such as a browser or a local application).

## Why Classic edition?
Classic is the original edition, it has all the standard features (although the simpler version, the rule builder is not included since it was added with fiddler everywhere)
Classic also only works on Windows (Fiddler Everywhere works on Mac and Linux)

## Why do we need to change response
The most common reason I want to change a response is where we cannot simulate a specific http code due to the backend functionality not been implemented yet.\
Modifying responses using a tool like Fiddler can serve various purposes in the context of testing. Here are some reasons why you might want to change responses:
1. **Testing Error Handling:** You can simulate error scenarios by modifying responses to include error codes, messages, or unexpected behaviour. This helps you ensure that your application handles errors gracefully.
2. **Testing Edge Cases:** Modifying responses lets you test how your application reacts to unusual or unexpected data. This can help uncover vulnerabilities or unexpected behaviours.
3. **Performance Testing:** You can simulate different response times or data sizes to assess how your application performs under various conditions.
4. **API Mocking:** During development or testing, you might not have access to the actual backend services. Modifying responses allows you to mimic the behaviour of those services and test different scenarios.
5. **Security Testing:** You can modify responses to test how your application responds to potential security vulnerabilities, such as SQL injection or cross-site scripting (XSS).
6. **Load Testing:** By modifying responses, you can create different load scenarios to assess how your application scales and performs under heavy usage.

## Manually changing requests
I find it best to exclude request that I am not interested, Do this beforehand.\
Enable Automatic breakpoints (Rules - Automatic Breakpoints - "Before Request" or "After Responses").\
All request that are paused will show up as red.
![Fiddler Paused Request](/assets/FiddlerPausedRequest.jpg)
After selecting the paused request, Go to Inspectors (default to the right hand side).\
Then there will the response options.
![Fiddler Response Options](/assets/FiddlerResponseOptions.jpg)
Remember to set Automatic Breakpoints back to disabled.

## Change request using scripting
Open Fiddler Script Editor (Rules - Customize Rules).\
Search for "OnBeforeRequest" or "OnBeforeResponse" (OnBeforeRequest is called before the request is sent to the server, while OnBeforeResponse is called after the response is received from the server).\
Add this code, Change (oSession.uriContains("/myapi/events") to the url that you want to change.
```powershell
if (oSession.uriContains("/myapi/events")){
    oSession["ui-color"]="orange"; 
    oSession["ui-bold"]="true";
    oSession.oRequest.FailSession(404, "Not Found", "Fiddler Custom Rule");
}
```
oSession.oRequest.FailSession fails the request and returns the status code(404 in this case).\
For a list of what can be changed please have a look here.\
<a href="https://docs.telerik.com/fiddler/knowledge-base/fiddlerscript/modifyrequestorresponse" target="_blank">Modify Request or Response</a>