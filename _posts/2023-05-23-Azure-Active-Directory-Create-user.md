---
title:  "Azure Active Directory: Create users"
#author: Jan Helberg
date:   2023-05-23 00:00:00 +0200
categories: [Powershell, Azure Active Directory]
tags: [Azure Active Directory, Powershell]
---

## Why I used this script?
I recently had to test a few scenarios using Azure Active directory users, I needed to create a predetermined number of users. This needed to be repeatable for future use

## Prerequisites
Powershell 2.0 \
Powershell Module (we can easily import this)
```powershell
Install-Module -Name AzureAD
```

## Create a predetermined number of users using Powershell
```powershell
#Get-Module AzureAD
Connect-AzureAD
$TotalUsers = 500 #Total number en new users
$UserPrefix = "AAD User" #User name prefix
$PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
$PasswordProfile.Password = "AccountPassword"
$AADDomain = Get-AzureADDomain | Where-Object Name -like "*mycompany.com*"
for($count = 1; $count -le $TotalUsers ; $count++)
{
    $DisplayName = $UserPrefix + $count
    $UserPrincipalName = $DisplayName + $AADDomain.Name
    $MailNickName = $DisplayName
    New-AzureADUser -DisplayName $DisplayName -PasswordProfile $PasswordProfile -UserPrincipalName $UserPrincipalName -AccountEnabled $true -MailNickName $MailNickName
}
```