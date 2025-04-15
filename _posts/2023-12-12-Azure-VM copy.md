---
title:  "Azure VM: Setup start to finish"
#author: Jan Helberg
date:   2030-08-05 00:10:00 +0200
categories: [Powershell, Azure]
tags: [Azure, Powershell, AzureVM, DSC]
---

## Why I used this script?
A while ago I had to setup a Virtual machine in Azure.\
I also needed to have a set up certain windows capabilities, I used DSC to set this up

## Prerequisites
Powershell 2.0 \
Azure Subscription

## Breakdown of the different steps
I broke down the setup into different steps, these was written as powershell functions
 - Function 1 : Connect to Azure
 - Function 2 : Create Resource Group
 - Function 3 : Provision Windows Server
 - Function 4 : Prep OS (Using DSC)
 - Function 5 : Save Details to notepad

## Powershell script
```powershell
# Function 1: Connect to Azure
function Connect-Azure {
    # Replace with your actual authentication method
    Connect-AzAccount
}

# Function 2: Create Resource Group
function Create-ResourceGroup {
    param (
        [string]$resourceGroupName,
        [string]$location
    )

    New-AzResourceGroup -Name $resourceGroupName -Location $location
}

# Function 3: Provision Windows Server VM
function Provision-WindowsVM {
    param (
        [string]$resourceGroupName,
        [string]$vmName,
        [string]$location,
        [string]$adminUsername,
        [string]$adminPassword,
        [string]$imageOffer,
        [string]$imagePublisher,
        [string]$imageSku
    )

    $cred = New-Object PSCredential -ArgumentList $adminUsername, ($adminPassword | ConvertTo-SecureString -AsPlainText -Force)

    $vmConfig = New-AzVMConfig -VMName $vmName -VMSize "Standard_DS2_v2"
    $vmConfig = Set-AzVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vmConfig = Set-AzVMSourceImage -VM $vmConfig -PublisherName $imagePublisher -Offer $imageOffer -Skus $imageSku -Version "latest"
    
    New-AzVM -ResourceGroupName $resourceGroupName -Location $location -VM $vmConfig
}

# Function 4: Prep OS using DSC (Desired State Configuration)
function Prep-OSUsingDSC {
    param (
        [string]$resourceGroupName,
        [string]$vmName
    )

    # Add DSC configuration code here
    $configuration = @{
        AllNodes = @(
            @{
                NodeName   = $vmName
                PSDscAllowPlainTextPassword = $true
            }
        )
    }

    $configurationData = @{
        AllNodes = @(
            @{
                NodeName = $vmName
                # Add more configuration data as needed
            }
        )
    }

    Start-DscConfiguration -Wait -Path $configurationPath -ConfigurationData $configurationData
}

# Function 5: Save Details to Notepad
function Save-DetailsToNotepad {
    param (
        [string]$filename,
        [string]$content
    )

    $content | Out-File -FilePath $filename -Append
}

# Main script

# Connect to Azure
Connect-Azure

# Create Resource Group
Create-ResourceGroup -resourceGroupName "MyResourceGroup" -location "East US"

# Provision Windows VM
Provision-WindowsVM -resourceGroupName "MyResourceGroup" -vmName "MyVM" -location "East US" -adminUsername "admin" -adminPassword "P@ssw0rd" -imageOffer "WindowsServer" -imagePublisher "MicrosoftWindowsServer" -imageSku "2016-Datacenter"

# Prep OS using DSC
Prep-OSUsingDSC -resourceGroupName "MyResourceGroup" -vmName "MyVM"

# Save Details to Notepad
Save-DetailsToNotepad -filename "InstallationDetails.txt" -content "Installation and configuration completed."

```