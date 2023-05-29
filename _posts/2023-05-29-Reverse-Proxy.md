---
title:  Reverse Proxy
#author: Jan Helberg
date:   2023-05-29 00:15:00 +0200
categories: [Software, Reverse Proxy]
tags: [Software, NGrok, CRProxy, NGinx]
---

## What is a Reverse Proxy?
In computer networks, a reverse proxy is an application that sits in front of back-end applications and forwards client requests to those applications. Reverse proxies help increase scalability, performance, resilience and security.
I use Reverse Proxy's to expose a program running locally (back-end applications) to the web (by forwarding client requests).

## NGRok
[NGrok](https://ngrok.com/) has been my go to reverse proxy for a while.
You need an authoken (which you get by creating a free account), to use the authtoken run:
```console
ngrok config add-authtoken 2BcH4FU....
```
Run using: 
```console
ngrok http https://localhost:5001 -host-header="localhost:5001"
```
NGrok allows you to inspect data, it also allows you to resend the data.
Open in browser: http://localhost:4040/

## CRProxy
[CRProxy](https://crproxy.com/) takes about the same time to get up and running as NGrok, you need a API Key, Instructions: 
https://crproxy.com/doc/quick-start/ 

Run using: 
```console
crproxy.exe proxy http://localhost:7071
```

## Nginx 
[Nginx](https://www.nginx.com/) seems to be a very good option, It has more community support (Walkthroughs wikis etc). It customisable and open source. Downside is the setup time.