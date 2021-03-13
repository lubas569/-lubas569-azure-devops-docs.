---
title: Azure DevOps - data location
description: Learn where your data is stored for Azure DevOps
ms.prod: devops
ms.topic: article
ms.technology: devops-security
ms.author: chcomley
author: chcomley
ms.manager: mijacobs
ms.date: 01/22/2020
monikerRange: 'azure-devops'
---

# Data locations for Azure DevOps

[!INCLUDE [temp](../../includes/version-vsts-only.md)]

You can choose the location for your data during initial sign-up and creation of your organization. Azure DevOps operates in the following geographical locations (“geos”). 

## Data locations

Azure DevOps data is available in the following eight geographies across the world: 

- Australia
- Brazil
- Canada
- East Asia
- Europe
- India
- United Kingdom
- United States

We default your organization to your closest region. However, you can choose a different region. Later on, if you change your mind, our CSS team can help you migrate your organization to a different region. 

## Customer data

Except as noted below, Azure DevOps maintains all customer data within your selected geography. Customer data includes the following data types:
- source code
- work items
- test results
- geo-redundant mirrors and offsite backups

Azure DevOps works with and uses many Microsoft Azure services. For details on customer data retention by location, see the [Azure data center map](https://azuredatacentermap.azurewebsites.net/).

### Profile data 
Azure DevOps stores information that's global in nature, such as user identities and profile information as follows: 
- EU-based users: profile data is in EU data center  
- US-based users: profile data is in US data center 
- Users from all other countries: profile data is in US data center 

## Transferring your data

Except as noted below, Microsoft doesn't transfer customer data outside of your selected region. 

If needed, you can transfer your data using preview, beta, or other pre-release services. These services typically store your data in the United States, but may store it globally.

> [!NOTE]
Microsoft will tranfer you data if it needs to do any of the following actions:
>- provide customer support
>- troubleshoot the service
>- comply with legal requirements

> [!NOTE]
> Microsoft doesn't control or limit the regions from which you or your users may access your data.

> [!NOTE]
> Because there's only one region in Brazil, customer data is replicated to south-central United States for disaster recovery and load balancing purposes. For more information, see the [Azure data center map](https://azuredatacentermap.azurewebsites.net/).

> [!NOTE]
> For builds and releases running on Microsoft-provided macOS agents, your data will be transferred to third party data centers as follows:
>- US-based users: your data is transferred to a third party US data center
>- Users from all other countries: your data is transferred to either a third party US data center or European data center
This data center is owned and managed by a third party with information security certification assurances, such as ISO 27001 and SOC 2 report.


## Related articles

- [Get started with Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137)
- [Data protection overview](data-protection.md)

