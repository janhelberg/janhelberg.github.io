---
title:  "Postman: Mock Server"
#author: Jan Helberg
date:   2023-09-12 22:30:00 +0200
categories: [Software, Postman]
tags: [Software, Postman, Mock, Mock Server]
---

## What is a Mock Server?
A mock server is a software tool that simulates the behaviour of a real server. It is used in software development and testing to isolate the system under test from external dependencies.

## Why use a Mock Server?
A mock server allows developers and testers to test their code without having to worry about the availability or reliability of the real server.

## Advantages of using a mock server?
1. Isolate the system under test
2. Improve testability
3. Reduce the risk of bugs
4. Speed up testing
5. Simulate different scenarios
6. Testing before development is done

## Enable mock servers
Click on Mock server in the workspace sidebar.\
Note: If Mock server does not show in the sidebar, click Configure workspace sidebar, and enable Mock Server.\
![PostmanEnableMockServer](/assets/PostmanEnableMockServer.jpg)

## Create a collection
Create a postman collection.\
![PostmanEmptyCollection](/assets/PostmanEmptyCollection.jpg)

## Create Mock Server
Go to the Mock server and create a new mock server.\
Select the previously created collection.\
After creating the mock server take note of the Mock Server URL.\
![PostmanCreateMockServer](/assets/PostmanCreateMockServer.jpg)
![PostmanMockServerCreated](/assets/PostmanMockServerCreated.jpg)

## Add Request and Example
Create a Request under collection, Use the Mock Server URL in the request.\
Right click the Request and "Add example".\
![PostmanCreateMockServer](/assets/PostmanNewRequest-AddExample.jpg)
Add the body.\
It should look like this:\
![PostmanCreateMockServer](/assets/PostmanAddBodyToExample.jpg)
You can also add Headers and the status code.\
![PostmanCreateMockServer](/assets/PostmanAddBodyHeaderStatusCodeToExample.jpg)

## Execute
Execute the request.\
Should return the details you entered in the example.\
![PostmanCreateMockServer](/assets/PostmanExecuteSuccess.jpg)