---
title:  "Active Directory: Create Users and groups"
#author: Jan Helberg
date:   2023-05-21 01:00:00 +0200
categories: [Powershell, Active Directory]
tags: [Active Directory, Powershell]
---

## Why I used these scripts?
I recently had to test a few scenarios using users and groups (The user names and group names didn't matter), I needed to create a predetermined number of users and groups, and this needed to be repeatable for future use (or for an environment where we would **not** need these users to be permanent and can remove them again)

## Prerequisites
Powershell 2.0 \
Powershell Module (we can easily import this)
```powershell
Import-Module ActiveDirectory
```
## Create a predetermined number of users using Powershell
```powershell
$TotalUsers = 500 #Total number of new users
$UserPrefix = "NewEmployee Number" #Username prefix
$OuPath = "OU=Call Centre," + (Get-ADDomain).DistinguishedName
For ($i=1; $i -le $TotalUsers; $i++)
{
    $NewUser = @{
    Name = $UserPrefix + $i
    AccountPassword = (Read-Host -AsSecureString 'AccountPassword')
    Path = $OuPath
    Enabled = $true
    }
    New-ADUser @NewUser
}
```

## Create a predetermined number of groups using Powershell
```powershell
$TotalGroups = 500 #Total number of new Groups
$GroupPrefix = "ZGroup" #Group name prefix
$OuPath = "OU=Call Centre," + (Get-ADDomain).DistinguishedName
For ($i=1; $i -le $TotalGroups; $i++)
{
    $GroupName = $GroupPrefix + $i
    New-ADGroup -Name $GroupName -SamAccountName $GroupName -GroupCategory Security -GroupScope Global -DisplayName $GroupName -Path $OuPath -Description $GroupName
}
```

## Remove the created users using Powershell
```powershell
Get-ADUser -Filter 'Name -like "NewEmployee Number*"' | Remove-ADUser
```

## Remove the created groups using Powershell
```powershell
Get-ADGroup -Filter 'Name -like "ZGroup*"' | Remove-ADGroup
```