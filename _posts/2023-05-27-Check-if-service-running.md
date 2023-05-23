---
title:  "Check if service is running"
#author: Jan Helberg
date:   2023-05-27 20:00:00 +0200
categories: [Windows, Service]
tags: [Windows, Powershell, Service]
---

## Why I used this script?
I used a program that was reliant on a service running, unfortunately the service would crash, I was not able to get a fix for the crash, so instead I wrote a powershell script to check if the service is running, and then give the option to start it.

## Prerequisites
Powershell 2.0

## Powershell script
```powershell
$ServiceName = 'Random Service'
$CheckService = Get-Service -Name $ServiceName
If($CheckService.Status -ne 'Running')
  {
      Write-Host Randome Service is not running
      $StartService = Read-Host "Start Random Service? [y/n]"
      if($StartService -eq "y"){
          Start-Service $ServiceName
          Write-Host Random Service starting
          Read-Host("Press Enter to Exit")
          Break; 
      }
  }
else
{
    Write-Host Random Service IS running
    Read-Host("Press Enter to Exit")
    Break; 
}
```