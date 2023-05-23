---
title:  "Service Fabric: Write Health to text file"
#author: Jan Helberg
date:   2023-05-28 20:00:00 +0200
categories: [Powershell,Service Fabric]
tags: [Service Fabric, Powershell, Health]
---

## Why I used this script?
I had set up service fabric locally and deployed an application to it. This script writes the errors and info to SFHealth.txt.

## Prerequisites
Powershell 2.0 \
Azure Subscription \
Service Fabric setup locally

## Powershell scripts
```powershell
Connect-ServiceFabricCluster
$ErrorFilter = [System.Fabric.Health.HealthStateFilter]::Error
$AllFilter = [System.Fabric.Health.HealthStateFilter]::All
$DspFilter1 = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$AllFilter}
$DaFilter1 =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$AllFilter;NodeNameFilter="N0020"}
$DaFilter1.DeployedServicePackageFilters.Add($DspFilter1)
$AppFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$AllFilter}
$AppFilter.DeployedApplicationFilters.Add($DaFilter1)
$AppFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$AppFilters.Add($AppFilter)
$currentDateTime = Get-Date
$text = Get-ServiceFabricClusterHealthChunk -ApplicationFilters $AppFilters
Add-Content \SFHealth.txt $currentDateTime
Add-Content \SFHealth.txt $text
$AppTypeHealthPolicyMap = New-Object -TypeName "System.Fabric.Health.ApplicationTypeHealthPolicyMap"
$AppTypeHealthPolicyMap.Add("CriticalAppType", 0)
$text = Get-ServiceFabricClusterHealth -ApplicationTypeHealthPolicyMap $AppTypeHealthPolicyMap -MaxPercentUnhealthyApplications 20
Add-Content \SFHealth.txt $text
```