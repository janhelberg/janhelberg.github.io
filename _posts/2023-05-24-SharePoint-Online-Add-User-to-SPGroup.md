---
title:  "SharePoint Online: Add users to a SharePoint Group"
#author: Jan Helberg
date:   2023-05-24 00:00:00 +0200
categories: [Powershell, Azure Active Directory]
tags: [Azure Active Directory, Powershell, SharePoint Online]
---

## Why I used this script?
I recently had to test a scenario using users that are in a SharePoint Online group. This needed to be repeatable for future use

## Prerequisites
Powershell 2.0 \
Powershell Modules (we can easily import this)
```powershell
Install-Module -Name AzureAD
Install-Module -Name Microsoft.Online.SharePoint.PowerShell
```

## Add Users to a SharePoint Group
```powershell
$Credential = Get-credential
$Url = "https://mySharePoint.sharepoint.com"
$Group = "SharePointGroup"
$Site = $url + "/sites/mySharePointSite"
Connect-SPOService -Url $Url -Credential $Credential
Connect-AzureAD
$useradd = Get-AzureADUser -All $true | select userprincipalname,objectid | where {$_.UserPrincipalName -like "AAD User*"}
$users = $useradd.userprincipalname
$users.Count
foreach($user in $users){
$user
Add-SPOUser -Site $Site -Group $Group -LoginName $user
}
```