---
title:  "Postman: Command Line"
#author: Jan Helberg
date:   2030-09-21 22:30:00 +0200
categories: [Software, Postman]
tags: [Software, Postman, Newman, CLI, CI/CD]
---

## What is a Newman?
Newman is a command-line collection runner for Postman. It enables you to run and test a Postman Collection directly from the command line. It's built with extensibility in mind so that you can easily integrate it with your continuous integration (CI) servers and build systems.

## How to install newman?
Install Newman.\
```powershell
npm install -g newman
```
Install Reporting.\
```powershell
npm install -g newman-reporter-html
```
## Running tests
```powershell
newman run mycollection.json -e dev_environment.json
```

## Reference
https://learning.postman.com/docs/collections/using-newman-cli/command-line-integration-with-newman/
https://learning.postman.com/docs/collections/using-newman-cli/installing-running-newman/