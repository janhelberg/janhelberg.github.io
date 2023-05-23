---
title:  "SQL: Database Lock"
#author: Jan Helberg
date:   2023-05-26 20:00:00 +0200
categories: [Powershell, SQL]
tags: [SQL Server, Powershell, Database Lock]
---

## Why I used this script?
A while ago a customer had a large table and would randomly encouter deadlocks, To try reproduce the deadlocks I wrote a powershell script to lock the table at random time for a random time.

## Prerequisites
Powershell 2.0 \
SQL Server
Database

## Powershell script
```powershell
#Enter amount of times to loop(Set HIGH for long running testing)
$loopcount = 10
for($i=1; $i -le $loopcount; $i++)
{
    #Enter the sleep duration in seconds
    $SleepDuration = Get-Random -Minimum 1 -Maximum 10
    Write-Host "Sleeping for :" $SleepDuration " seconds"
    Start-Sleep -s $SleepDuration
    #Enter the lock duration in seconds
    $LockDuration = Get-Random -Minimum 1 -Maximum 10
    Write-Host "Locking for :"$LockDuration " seconds"
    $ts =  [timespan]::fromseconds($LockDuration)
    #Enter the Table to lock(Remember to include database ie [MyDB].[dbo].[Customers]
    $LockSchemaTable = '[MyDB].[dbo].[Customers]'
    $SQLQuery = "BEGIN TRANSACTION SELECT COUNT(*) FROM "+ $LockSchemaTable +" WITH (TABLOCKX, HOLDLOCK) WAITFOR DELAY '" + $ts + "' ROLLBACK TRANSACTION"
    Write-Host $SQLQuery
    Invoke-Sqlcmd -Query $SQLQuery
}
```