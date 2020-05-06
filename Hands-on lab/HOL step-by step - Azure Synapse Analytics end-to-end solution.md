![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Azure Synapse Analytics end-to-end solution
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
May 2020
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2020 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents** 

<!-- TOC -->
- [Azure Synapse Analytics end-to-end solution hands-on lab step-by-step](#azure-synapse-analytics-end-to-end-solution-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
  - [Resource naming throughout this lab](#resource-naming-throughout-this-lab)
  - [Exercise 1: Accessing the Azure Synapse Analytics workspace](#exercise-1-accessing-the-azure-synapse-analytics-workspace)
    - [Task 1: Launching Synapse Studio](#task-1-launching-synapse-studio)
  - [Exercise 2: Create and populate customer information and sales tables in the SQL Pool](#exercise-2-create-and-populate-customer-information-and-sales-tables-in-the-sql-pool)
    - [Task 1: Create an HTTP linked service to obtain raw data](#task-1-create-an-http-linked-service-to-obtain-raw-data)
    - [Task 3: Create and populate the customer information table](#task-3-create-and-populate-the-customer-information-table)
    - [Task 4: Create and populate the sales table](#task-4-create-and-populate-the-sales-table)
  - [Exercise 5 - Security](#exercise-5---security)
    - [Task 1 - Column level security](#task-1---column-level-security)
    - [Task 2 - Row level security](#task-2---row-level-security)
    - [Task 3 - Dynamic data masking](#task-3---dynamic-data-masking)
  - [After the hands-on lab](#after-the-hands-on-lab)
<!-- /TOC -->

# Azure Synapse Analytics end-to-end solution hands-on lab step-by-step

## Abstract and learning objectives

In this hands-on-lab, you will build an end-to-end data analytics with machine learning solution using Azure Synapse Analytics. The information will be presented in the context of a retail scenario. We will be heavily leveraging Azure Synapse Studio, a tool that conveniently unifies the most common data operations from ingestion, transformation, querying, and visualization.

## Overview

TODO: at the end make sure we include an outline of features used in this lab

## Solution architecture

TODO: at the end make sure we include diagram of all features of this lab

## Requirements

1. Microsoft Azure subscription

## Before the hands-on lab

Refer to the Before the hands-on lab setup guide manual before continuing to the lab exercises.

## Resource naming throughout this lab

For the remainder of this guide, the following terms will be used for various ASA-related resources (make sure you replace them with actual names and values from your environment):

| Azure Synapse Analytics Resource  | To be referred to                                                                  |
|-----------------------------------|------------------------------------------------------------------------------------|
| Azure Subscription                | `WorkspaceSubscription`                                                            |
| Azure Region                      | `WorkspaceRegion`                                                                  |
| Workspace resource group          | `WorkspaceResourceGroup`                                                           |
| Workspace / workspace name        | `Workspace`                                                                        |
| Primary Storage Account           | `PrimaryStorage`                                                                   |
| Default file system container     | `DefaultFileSystem`                                                                |
| SQL Pool                          | `SqlPool01`                                                                        |
| SQL Serverless Endpoint           | `SqlServerless01`                                                                  |
| Azure Key Vault                   | `KeyVault01`                                                                       |

## Exercise 1: Accessing the Azure Synapse Analytics workspace

All exercises in this lab utilize the workspace Synapse Studio user interface. This exercise will outline the steps to launch Synapse Studio. Unless otherwise specified, all instruction including menu navigation will occur in Synapse Studio.

### Task 1: Launching Synapse Studio

1. Log into the ![Azure Portal](https://portal.azure.com).

2. Expand the left menu, and select the **Resource groups** item.
  
    ![The Azure Portal left menu is expanded with the Resource groups item highlighted.](media/azureportal_leftmenu_resourcegroups.png)

3. From the list of resource groups, select `WorkspaceResourceGroup`.
  
4. From the list of resources, select the **Synapse Workspace** resource, `Workspace`.
  
    ![In the resource list, the Synapse Workspace item is selected.](media/resourcelist_synapseworkspace.png)

5. On the **Overview** tab of the Synapse Workspace page, select the **Launch Synapse Studio** item from the top toolbar. Alternatively you can select the Workspace web URL link.

    ![On the Synapse workspace resource screen, the Overview pane is shown with the Launch Synapse Studio button highlighted in the top toolbar. The Workspace web URL value is also highlighted.](media/workspaceresource_launchsynapsestudio.png)

## Exercise 2: Create and populate customer information and sales tables in the SQL Pool

Duration: X minutes

The first step in querying meaningful data is to create tables to house the data. In this case, we will create two different tables: CustomerInfo and Sales. When designing tables in Azure Synapse Analytics, we need to take into account the expected amount of data in each table, as well as how each will be used. Utilize the following guidance when designing your tables to ensure the best experience and performance.

Table design performance considerations

| Table Indexing | Recommended use |
|--------------|-------------|
| Clustered Columnstore | recommended for tables with greater than 100 million rows, offers the highest data compression with best overall query performance |
| Heap Tables | Smaller tables with less than 100 million rows, commonly used as a staging table prior to transformation |
| Clustered Index | large lookup tables (> 100 million rows) where querying will only result in a single row returned |
| Clustered Index + non-clustered secondary index | large tables (> 100 million rows) when single (or very few) records are being returned in queries |

| Table Distribution | Recommended use |
|--------------------|-------------|
| Hash distribution | tables that are larger than 2 GBs with infrequent insert/update/delete operations, works well for large fact tables in a star schema |
| Round robin distribution | default distribution, when little is known about the data or how it will be used. Use this distribution for staging tables |
| Replicated tables | smaller lookup tables less than 1.5 GB in size |

### Task 1: Create an HTTP linked service to obtain raw data

Linked Services are synonymous with connection strings in Azure Synapse Analytics. Azure Synapse Analytics linked services provides the ability to connect to nearly 100 different types of external services ranging from Azure Storage Accounts to Amazon S3 and more. In this exercise, we will be pulling raw data that is housed in CSV format from a public HTTP endpoint. We will need to create a Linked Service to define the location where this data will be obtained.

1. Expand the left menu and select the **Manage** item.

    ![In Synapse Studio, the left menu is expanded and the Manage item is selected.](media/synapsestudio_leftmenu_manage.png)

2. From the secondary menu, beneath **External connections** select **Linked services**.

    ![The Manage menu is displayed with the Linked services item selected beneath the External connections section.](media/synapsestudio_managemenu_linkedservices.png)

3. On the **Linked services** screen, select **+ New** from the toolbar menu.

    ![The top portion of the Linked services screen is displayed with the + New button selected from the toolbar menu.](media/synapsestudio_linkedservices_toolbar_new.png)

4. On the **New linked service** blade, select the **File** tab and the **HTTP** item; then select **Continue**.

    ![The New linked service blade is shown with the File tab selected and the HTTP item selected from the filtered results.](media/newlinkedservice_file_http.png)

5. On the **New linked service (HTTP)** blade, fill the form as follows, then select **Create**:
  
   | Field | Value |
   |-------|-------|
   | Name  | Enter `asamcw_publicdata` |
   | Base URL | Enter `https://solliancepublicdata.blob.core.windows.net/` |
   | Authentication type | Select **Anonymous** |

   ![The New linked service (HTTP) blade is displayed populated with the values from the previous table.](media/newlinkedservicehttp_form.png)

6. On the **Manage** screen toolbar, you should now see one change that hasn't been published. Select the **Publish all** button to publish the new linked service.

    ![The Manage screen is displayed with the Publish all button selected from the toolbar. There is a badge on the Publish all button indicating 1 change is pending.](media/linkedservice_publishall_toolbar.png)

7. In the Publish all blade, select the **Publish** button to commit the changes.

### Task 3: Create and populate the customer information table

Over the past 5 years, Wide World Importers has amassed over 3 billion rows of sales data. With this quantity of data, the customer information lookup table is estimated to have over 100 million rows but will consume less than 1.5 GB of storage. While we will be using only a subset of this data for the lab, we will design the table for the production environment. Using the guidance outlined in the task description, we can ascertain that we will need a **Clustered Columnstore** table with a **Replicated** table distribution.

### Task 4: Create and populate the sales table

## Exercise 5 - Security

### Task 1 - Column level security

### Task 2 - Row level security

### Task 3 - Dynamic data masking

## After the hands-on lab

Duration: X minutes

You should follow all steps provided *after* attending the Hands-on lab.
