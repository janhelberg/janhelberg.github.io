---
title:  Desired State configuration
#author: Jan Helberg
date:   2023-05-20 00:15:00 +0200
categories: [Technologies]
tags: [Software]
---

## What is Desired State configuration (DSC)
Desired State Configuration (DSC) is a feature in PowerShell 4.0 and above that helps administrators to automate the configuration of operating systems (Windows and Linux).

## DSC vs Group Policy
#### Definition of Group Policy
Group Policy (GP) is a Windows management feature that allows you to control multiple users' and computers' configurations within an Active Directory environment.
DSC can imitate GPO by setting the exact same registry keys and values. DSC can be used without Active directory, making it easier to use for once off deployments.

## DSC vs Custom Scripts (Powershell or other)
Why not just use Custom Scripts? We can write custom scripts, but these scripts have to be very specific and don't always function the same across different version of operating systems and software, DSC tells the operating system and/or software what is required, and then it is up to the operating system/software to do the work necessary to accomplish this.

## Push vs Pull
DSC can work in two different ways, Push and Pull
In the Push mode, the configuration is pushed manually on target nodes and it is unidirectional. In the “Pull” mode, however, nodes are configured to get their desired configuration from the 'pull' server.
For testing purposes we generally don't want to setup a server as a "pull server", there is other solution that would be better if this is needed.

## Why use DSC?
When we have legacy system/s that require a full Windows/Linux operating system but we don't need to run it constantly DSC can help to get servers up quicker.