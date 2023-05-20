---
title:  "Active Directory: Import Users from CSV"
#author: Jan Helberg
date:   2023-05-22 00:00:00 +0200
categories: [Powershell, Active Directory]
tags: [Active Directory, Powershell]
---

## Why I used these scripts?
I recently had to test a few scenarios using users and groups, I needed to create a predetermined number of users and groups (where I needed to create the users from a set list of names), and this needed to be repeatable for future use (or for an environment where we would **not** need these users to be permanent and can remove them again)

## Pre-requisites
Powershell 2.0 \
Powershell Module (we can easily import this)
```powershell
Import-Module ActiveDirectory
```

## Create an Active Directory users using Powershell from Users defined in a CSV File
All these users needed to be in the same Active directory path "OU=Call Centre"
They needed to be in the same group ("Call Centre")
I also needed all these users to me managed by a single users, For ease of use I created the user in the script (Teressa Watts). 
```powershell
Import-Module ActiveDirectory
$filename = "C:\Users\Administrator\Desktop\New-Users100000.csv"
$path = "OU=Call Centre," + (Get-ADDomain).DistinguishedName
Write-Host "Importing Active Directory"
Write-Host "Starting User creation"
New-ADOrganizationalUnit -Name "Call Centre"
#New User to act as a manager for other users
New-ADUser -Name "Teressa Watts" -AccountPassword (ConvertTo-SecureString 'MySecurePassword!' -AsPlainText -force) -Enabled $true -Path $path
#New Group for the new users
New-ADGroup -Name "Call Centre" -ManagedBy "Teressa Watts" -Path $path -GroupScope DomainLocal
Import-Csv $filename | ForEach-Object {
$uname = $_.LastName + " " + $_.FirstName
$samaccountname = $_.FirstName + " " + $_.LastName 
$upn = $samaccountname + “@mycallcentre.com”
New-ADUser -Name $uname `
-DisplayName $uname `
-GivenName $_.FirstName `
-Surname $_.LastName `
-Department "Call Centre" `
-Title "Call Centre Agent" `
-UserPrincipalName $upn `
-SamAccountName $samaccountname `
-Path $path `
-Manager "Teressa Watts" `
-AccountPassword (ConvertTo-SecureString 'MySecurePassword!' -AsPlainText -force) -Enabled $true
Add-ADGroupMember -Identity "Call Centre" -Members $samaccountname
}
Write-Host "Done"
```

## How should the data be saved in the csv file
This data needs to have a heading so we can pick the data based on the column
```
Name,DisplayName,GivenName,Surname,SamAccountName,UserPrincipalName,EmailAddress,Description,Manager,Department
Aaden Bartlett,Aaden Bartlett,Aaden,Bartlett,AadenBartlett,AadenBartlett@myemail.com,AadenBartlett@myemail.com,Call centre Agent,TeresaWatts,
Aaden Estes,Aaden Estes,Aaden,Estes,AadenEstes,AadenEstes@myemail.com,AadenEstes@myemail.com,Call centre Agent,TeresaWatts,
etc etc etc
```