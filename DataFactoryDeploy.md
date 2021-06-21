# Deploying Insight Data factory pipelines 

  

## Contents 

  

- [Prerequisites](#Prerequisites) 

- [Deploying using PowerShell](#Deploying-using-PowerShell) 

- [Deploying Additional Datasets and Pipelines [Unsupported]](#Deploying-Additional-Datasets-and-Pipelines-[Unsupported]) 

  

## Prerequisites 

  

#### Local Environment (Your Machine) 

  

- PowerShell v7.x or later ([Latest Release](https://github.com/PowerShell/PowerShell/releases/latest)) 

- Azure PowerShell modules ([Installation Instructions](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps)) 

    

##### Check Your Installation: 

  

    # Get PowerShell Version 

    $PSVersionTable.PSVersion 

  

    # Get Azure PowerShell Module Version 

    Get-InstalledModule -Name Az 

  

##### Sign in:  

  

    # Connect to Azure with a browser sign in token 

    Connect-AzAccount 

  

#### Target Environment (Azure) 

  

Before Deploying Data Warehouse you should ensure the following requirements are met: 

  

- The `Subscription` and `Resource Group` you wish to deploy to already exist 

- There is a `Key Vault` instance that contains the Database Connection Strings your Data Warehouse requires as KeyVault Secrets 

##### Key Vault 

Data Warehouse establishes a Database Connection to one or more Source Databases. Each Database is added to Data Warehouse as a **Linked Service**. Each **Linked Service** gets its Connection String via Key Vault Reference. 

  

Within your **Key Vault** instance you should have a Connection String for each Linked Service you require


## Deploying using PowerShell 

  

Data WareHouse can be deployed using the [deploy/deployDataWarehouse.ps1 PowerShell Script](deploy/deployDataWarehouse.ps1). 

> The same script can also be used to upgrade an existing Data Warehouse instance - just be mindful that it will only overwrite things which already exist, or create those that do not - it will not remove things which are no longer part of the Deploy.  

> 

> For example, if you initially Deployed for both RM and FITS, performing an upgrade later for just RM will leave the original FITS configuration intact. 

  

The script requires the following parameters: 

  

| Parameter | Type | Mandatory | Description | Default | Example |
|-----------|------|-----------|-------------|---------|--------| 
| SubscriptionName | string | Yes |The name of the Subscription as shown in the Azure Portal | - | 'trg-optimize-dev' | 
| ResourceGroupName| string | Yes | The name of the Resource Group as shown in the Azure Portal | - | rg-shared-dev-global | 
| FactoryName | string | Yes | The name of the new or existing Data Warehouse you wish to Deploy | - | dev-inisght-df-eastus-test | 
| KeyVaultName | string | Yes | The name of the Key Vault instance containing your Connection Strings | - | kvdevoptimize | 
| Features | string[] | Yes | The list of Features you wish to Deploy: 'FITS', 'RM' | - | FITS,RM | 
| CustomerName | string | Yes | The name of the Customer you wish to Deploy | - | 'Morgan Stanley' | 
| TenantId | guid | Yes | The Tenant Id of the Customer you wish to Deploy | - | 79cdeb81-5b64-4047-9cf7-127d9a941f7a  | 
| ProductVersion | string | No | The Version of Product | 1.0 | '2020.10.1' | 
| TriggerFrequency | string | No | The Frequency the Trigger should run at: `Minute`, `Hour`, `Day`, `Week`, `Month` | Day | Week | 
| TriggerInterval | int | No | The Interval on which the Trigger should iterate. If the TriggerFrequency is `Minute` and the TriggerInteral is 15, the Trigger will fire every 15 Minutes | 1 | 15 | 
| TriggerStartTime | DateTime | No | The time the Trigger should start running from. | 2020-01-01T18:00:00 | 2020-11-11T23:00:00 | 

Examples: 

  

    # Deploy all Features 

     ./deployDataWarehouse.ps1 'trg-optimize-dev' rg-shared-dev-global dev-inisght-df-eastus-test kvdevoptimize RM 'RM' 69c29783-6568-4c57-9fe2-ab6649d00b91

  

    # Deploy just RM 

    ./deployDataWarehouse.ps1 'trg-optimize-dev' rg-shared-dev-global dev-inisght-df-eastus-test kvdevoptimize RM 'RM' 69c29783-6568-4c57-9fe2-ab6649d00b91

  

    # Deploy with custom Product Version number 

    ./deployDataWarehouse.ps1 'trg-optimize-dev' rg-shared-dev-global dev-inisght-df-eastus-test kvdevoptimize RM 'RM' 69c29783-6568-4c57-9fe2-ab6649d00b91 2020.11.1 

  

    # Deploy with a custom Trigger configuration 

   ./deployDataWarehouse.ps1 'trg-optimize-dev' rg-shared-dev-global dev-inisght-df-eastus-test kvdevoptimize RM 'RM' 69c29783-6568-4c57-9fe2-ab6649d00b91 2020.11.1 Minute 10 '2020-11-01T23:30:00' 

  







  
