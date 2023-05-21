---
title:  "Azure Active Directory: Rotate group membership"
#author: Jan Helberg
date:   2023-05-25 00:00:00 +0200
categories: [Powershell, Azure Active Directory]
tags: [Azure Active Directory, Powershell]
---

## Why I used this script?
I had an interesting scenario a while back where I would need to test membership changes, I even had to go as far as repeatedly changing the membership and to accomplish this I put the below into a loop.

## Prerequisites
Powershell 2.0 \
Powershell Module (we can easily import this)
```powershell
Install-Module -Name AzureAD
```
## Change 
```powershell
Connect-AzureAD
$Group1ObjectID = '70917518-9b22-4ff2-a10b-fad00b1929d2'
$Group2ObjectID = 'b5e782ce-1ae6-4ab3-8c5a-2d4be7b7adbd'

$Group1Members = Get-AzureADGroupMember -ObjectId $Group1ObjectID
$Group2Members = Get-AzureADGroupMember -ObjectId $Group2ObjectID

foreach($Group1Member in $Group1Members)
{
    Remove-AzureADGroupMember -ObjectId $Group1ObjectID -MemberId $Group1Member.ObjectId
    Add-AzureADGroupMember -ObjectId $Group2ObjectID -RefObjectId $Group1Member.ObjectId
}
foreach($Group2Member in $Group2Members)
{
    Remove-AzureADGroupMember -ObjectId $Group2ObjectID -MemberId $Group2Member.ObjectId
    Add-AzureADGroupMember -ObjectId $Group3ObjectID -RefObjectId $Group2Member.ObjectId
}
```