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
  - [Exercise 2: Create and populate the supporting tables in the SQL Pool](#exercise-2-create-and-populate-the-supporting-tables-in-the-sql-pool)
    - [Task 1: Create the customer information table](#task-1-create-the-customer-information-table)
    - [Task 2: Populate the customer information table](#task-2-populate-the-customer-information-table)
    - [Task 3: Create the campaign analytics table](#task-3-create-the-campaign-analytics-table)
    - [Task 4: Populate the campaign analytics table](#task-4-populate-the-campaign-analytics-table)
    - [Task 5: Create the sale table](#task-5-create-the-sale-table)
    - [Task 6: Populate the sale table](#task-6-populate-the-sale-table)
  - [Exercise 5: Security](#exercise-5-security)
    - [Task 1: Column level security](#task-1-column-level-security)
    - [Task 2: Row level security](#task-2-row-level-security)
    - [Task 3: Dynamic data masking](#task-3-dynamic-data-masking)
  - [Exercise 7: Monitoring](#exercise-7-monitoring)
    - [Task 1: Workload Importance](#task-1-workload-importance)
    - [Task 2: Workload Isolation](#task-2-workload-isolation)
    - [Task 3: Monitoring with Dynamic Management Views](#task-3-monitoring-with-dynamic-management-views)
    - [Task 4: Orchestration Monitoring with the Monitor Hub](#task-4-orchestration-monitoring-with-the-monitor-hub)
    - [Task 5: Monitoring SQL Requests with the Monitor Hub](#task-5-monitoring-sql-requests-with-the-monitor-hub)
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

For the remainder of this guide, the following terms will be used for various ASA (Azure Synapse Analytics) related resources (make sure you replace them with actual names and values from your environment):

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

1. Log into the [Azure Portal](https://portal.azure.com).

2. Expand the left menu, and select the **Resource groups** item.
  
    ![The Azure Portal left menu is expanded with the Resource groups item highlighted.](media/azureportal_leftmenu_resourcegroups.png)

3. From the list of resource groups, select `WorkspaceResourceGroup`.
  
4. From the list of resources, select the **Synapse Workspace** resource, `Workspace`.
  
    ![In the resource list, the Synapse Workspace item is selected.](media/resourcelist_synapseworkspace.png)

5. On the **Overview** tab of the Synapse Workspace page, select the **Launch Synapse Studio** item from the top toolbar. Alternatively you can select the Workspace web URL link.

    ![On the Synapse workspace resource screen, the Overview pane is shown with the Launch Synapse Studio button highlighted in the top toolbar. The Workspace web URL value is also highlighted.](media/workspaceresource_launchsynapsestudio.png)

## Exercise 2: Create and populate the supporting tables in the SQL Pool

Duration: X minutes

The first step in querying meaningful data is to create tables to house the data. In this case, we will create two different tables: CustomerInfo and Sales. When designing tables in Azure Synapse Analytics, we need to take into account the expected amount of data in each table, as well as how each table will be used. Utilize the following guidance when designing your tables to ensure the best experience and performance.

Table design performance considerations

| Table Indexing | Recommended use |
|--------------|-------------|
| Clustered Columnstore | recommended for tables with greater than 100 million rows, offers the highest data compression with best overall query performance |
| Heap Tables | Smaller tables with less than 100 million rows, commonly used as a staging table prior to transformation |
| Clustered Index | large lookup tables (> 100 million rows) where querying will only result in a single row returned |
| Clustered Index + non-clustered secondary index | large tables (> 100 million rows) when single (or very few) records are being returned in queries |

| Table Distribution/Partition Type | Recommended use |
|--------------------|-------------|
| Hash distribution | tables that are larger than 2 GBs with infrequent insert/update/delete operations, works well for large fact tables in a star schema |
| Round robin distribution | default distribution, when little is known about the data or how it will be used. Use this distribution for staging tables |
| Replicated tables | smaller lookup tables less than 1.5 GB in size |

### Task 1: Create the customer information table

Over the past 5 years, Wide World Importers has amassed over 3 billion rows of sales data. With this quantity of data, the customer information lookup table is estimated to have over 100 million rows but will consume less than 1.5 GB of storage. While we will be using only a subset of this data for the lab, we will design the table for the production environment. Using the guidance outlined in the Exercising description, we can ascertain that we will need a **Clustered Columnstore** table with a **Replicated** table distribution to hold customer data.

1. Expand the left menu and select the **Develop** item. From the **Develop** blade, expand the **+** button and select the **SQL script** item.

    ![The left menu is expanded with the Develop item selected. The Develop blade has the + button expanded with the SQL script item highlighted.](media/develop_newsqlscript_menu.png)

2. In the query tab toolbar menu, ensure you connect to your SQL Pool, `SQLPool01`.

    ![The query tab toolbar menu is displayed with the Connect to set to the SQL Pool.](media/querytoolbar_connecttosqlpool.png)

3. In the query window, copy and paste the following query to create the customer information table. Then select the **Run** button in the query tab toolbar.
  
   ```sql
    CREATE TABLE [wwi_mcw].[CustomerInfo]
    (
      [UserName] [nvarchar](100)  NULL,
      [Gender] [nvarchar](10)  NULL,
      [Phone] [nvarchar](50)  NULL,
      [Email] [nvarchar](150)  NULL,
      [CreditCard] [nvarchar](21)  NULL
    )
    WITH
    (
      DISTRIBUTION = REPLICATE,
      CLUSTERED COLUMNSTORE INDEX
    )
    GO
   ```

   ![The query tab toolbar is displayed with the Run button selected.](media/querytoolbar_run.png)

4. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)

### Task 2: Populate the customer information table

1. The data that we will be retrieving to populate the customer information table is currently stored in CSV format in a public blob storage container. The storage account that possesses this data has already been added as a linked service in Azure Synapse Analytics when the environment was provisioned.  Linked Services are synonymous with connection strings in Azure Synapse Analytics. Azure Synapse Analytics linked services provides the ability to connect to nearly 100 different types of external services ranging from Azure Storage Accounts to Amazon S3 and more. Review the presence of the **solliancepublicdata** linked service, by selecting **Manage** from the left menu, and selecting **Linked services** from the blade menu. Filter the linked services by the term **solliance** to find the **solliancepublicdata** item. Further investigating this item will unveil that it makes a connection to the storage account using a storage account key.
  
   ![The Manage item is selected from the left menu. The Linked services menu item is selected on the blade. On the Linked services screen the term solliance is entered in the search box and the solliancepublicdata Azure Blob Storage item is selected from the filtered results list.](media/manage_linkedservices_solliancepublicdata.png)

2. Similar to the previous step, the destination for our data has also been added as a linked service. In this case, the destination for our data is our SQL Pool, `SQLPool01`. Repeat the previous step, this time filtering with the term **sqlpool** to verify the existence of the linked service.

3. The next thing that we will need to do is define a source dataset that will represent the information that we are copying over. This dataset will reference the CSV file containing customer information. From the left menu, select **Data**. From the **Data** blade, expand the **+** button and select **Dataset**.

    ![The Data item is selected from the left menu. On the Data blade, the + button is expanded with the Dataset item highlighted.](media/data_newdatasetmenu.png)

4. On the **New dataset** blade, with the **All** tab selected, choose the **Azure Blob Storage** item. Select **Continue**.  
  
    ![On the New dataset blade, the All tab is selected and the Azure Blob Storage item is highlighted.](media/newdataset_azureblobstorage.png)

5. On the **Select format** blade, select **CSV Delimited Text**. Select **Continue**.

    ![On the Select format blade the CSV Delimited Text item is highlighted.](media/newdataset_selectfileformat_csv.png)

6. On the **Set properties** blade, set the fields to the following values, then select **OK**.

   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_customerinfo_csv** |
   | Linked service | Select **solliancepublicdata**.|
   | File Path - Container | Enter **wwi-02** |
   | File Path - Directory | Enter **customer-info** |
   | File Path - File | Enter **customerinfo.csv** |
   | First row as header | Checked |
   | Import schema | Select **From connection/store** |

    ![The Set properties form is displayed with the values specified in the previous table.](media/customerinfodatasetpropertiesform.png)

7. Now we will need to define the destination dataset for our data. In this case we will be storing customer information data in our SQL Pool. On the **Data** blade, expand the **+** button just as you did in **Step 3**.

8. On the **New dataset** blade, with the **Azure** tab selected, enter **synapse** as a search term and select the **Azure Synapse Analytics (formerly SQL DW)** item. Select **Continue**.

    ![On the New dataset blade, synapse is entered as the search term and Azure Synapse Analytics (formerly SQL DW) is selected from the filtered results.](media/newdataset_synapseitem.png)
  
9. On the **Set properties** blade, set the field values to the following, then select **OK**.

   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_customerinfo_asa** |
   | Linked service | Select `SQLPool01`. |
   | Table name | Select **wwi_mcw.CustomerInfo**. |  
   | Import schema | Select **From connection/store** |

    ![The Set properties blade is populated with the values specified in the preceding table.](media/dataset_customerinfoasaform.png)
  
10. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to commit the changes.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

11. Next, we will define a pipeline to populate data into the CustomerInfo table. From the left menu, select **Orchestrate**. From the Orchestrate blade, select the **+** button and select the **Pipeline** item.

    ![The Orchestrate menu item is selected from the left menu. On the Orchestrate blade, the + button is expanded with the Pipeline item highlighted.](media/orchestrate_newpipelinemenu.png)

12. In the bottom pane, on the **General** tab, enter **ASAMCW - Exercise 2 - Copy Customer Information** in the **Name** field.

    ![The General tab is shown with the name field populated as described above.](media/pipeline_customerinfo_generaltab.png)

13. In the **Activities** menu, expand the **Move & transform** item. Drag an instance of the **Copy data** activity to the design surface of the pipeline.

    ![In the Activities menu, the Move and transform section is expanded. An arrow denotes an instance of the Copy data activity being dragged over to the design surface of the pipeline.](media/pipeline_addcopydataactivity.png)

14. Select the **Copy data** activity on the pipeline design surface. In the bottom pane, on the **General** tab, enter **Copy Customer Information Data** in the **Name** field.

    ![The General tab is selected with the Name field set to Copy Customer Information Data.](media/pipeline_copycustomerinformation_general.png)

15. Select the **Source** tab in the bottom pane. In the **Source dataset** field, select **asamcw_customerinfo_csv**.

    ![The Source tab is selected with the Source dataset field set to asamcw_customerinfo_csv.](media/pipeline_copycustomerinformation_source.png)
  
16. Select the **Sink** tab in the bottom pane. In the **Sink dataset** field, select **asamcw_customerinfo_asa**, for the **Copy method** field, select **Bulk insert**, and for **Pre-copy script** enter:

    ```sql
      truncate table wwi_mcw.CustomerInfo
    ```

    ![The Sink tab is selected with the Sink dataset field set to asamcw_customerinfo_asa, the Copy method set to Bulk insert, and the Pre-copy script field set to the previous query.](media/pipeline_copycustomerinformation_sink.png)
  
17. Select the **Mapping** tab in the bottom pane. Select the **Import schemas** button. You will notice that Azure Synapse Analytics automated the mapping for us since the field names and types match.

    ![The Mapping tab is selected in the bottom pane. The source to destination field mapping is shown.](media/pipeline_copycustomerinformation_mapping.png)

18. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to commit the changes.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

19. Once published, expand the **Add trigger** item on the pipeline designer toolbar, and select **Trigger now**. In the **Pipeline run** blade, select **OK** to proceed with the latest published configuration. You will see notification toast windows indicating the pipeline is running and when it has completed.

20. View the status of the completed run by locating the **ASAMCW - Exercise 2 - Copy Customer Information** pipeline in the Orchestrate blade. Expand the actions menu, and select the **Monitor** item.

    ![In the Orchestrate blade, the Action menu is displayed with the Monitor item selected on the ASAMCW - Exercise 2 - Copy Customer Information pipeline.](media/pipeline_copycustomerinformation_monitormenu.png)
  
21. You should see a successful run of the pipeline we created in the **Pipeline runs** table.
  
    ![On the pipeline runs screen, a successful pipeline run is highlighted in the table.](media/pipeline_run_customerinfo_successful.png)

22. Verify the table has populated by creating a new query. Remember from **Task 1**, select the **Develop** item from the left menu, and in the **Develop** blade, expand the **+** button, and select **SQL script**. In the query window, be sure to connect to the SQL Pool database (`SQLPool01`), then paste and run the following query. When complete, select the **Discard all** button from the top toolbar.

  ```sql
    select * from wwi_mcw.CustomerInfo;
  ```
  
### Task 3: Create the campaign analytics table

1. Create the campaign analytics table

    ```sql
    CREATE TABLE [wwi_mcw].[CampaignAnalytics]
    (
        [Region] [nvarchar](50)  NOT NULL,
        [Country] [nvarchar](30)  NOT NULL,
        [ProductCategory] [nvarchar](50)  NOT NULL,
        [CampaignName] [nvarchar](500)  NOT NULL,
        [Analyst] [nvarchar](20) NOT NULL,
        [Revenue] [decimal](10,2)  NULL,
        [RevenueTarget] [decimal](10,2)  NULL,
        [City] [nvarchar](50)  NULL,
        [State] [nvarchar](25)  NULL
    )
    WITH
    (
        DISTRIBUTION = HASH ( [Region] ),
        CLUSTERED COLUMNSTORE INDEX
    );  
    ```

### Task 4: Populate the campaign analytics table

### Task 5: Create the sale table

Over the past 5 years, Wide World Importers has amassed over 3 billion rows of sales data. With this quantity of data, the storage consumed would be greater than 2 GB. While we will be using only a subset of this data for the lab, we will design the table for the production environment. Using the guidance outlined in the current Exercise description, we can ascertain that we will need a **Clustered Columnstore** table with a **Hash** table distribution based on the **CustomerId** field which will be used in most queries. For further performance gains, the table will be partitioned by transaction date to ensure queries that include dates or date arithmetic are returned in a favorable amount of time.

1. Expand the left menu and select the **Develop** item. From the **Develop** blade, expand the **+** button and select the **SQL script** item.

    ![The left menu is expanded with the Develop item selected. The Develop blade has the + button expanded with the SQL script item highlighted.](media/develop_newsqlscript_menu.png)

2. In the query tab toolbar menu, ensure you connect to your SQL Pool, `SQLPool01`.

    ![The query tab toolbar menu is displayed with the Connect to set to the SQL Pool.](media/querytoolbar_connecttosqlpool.png)

3. In the query window, copy and paste the following query to create the customer information table. Then select the **Run** button in the query tab toolbar.

    ```sql
      CREATE TABLE [wwi_mcw].[SaleSmall]
      (
        [TransactionId] [uniqueidentifier]  NOT NULL,
        [CustomerId] [int]  NOT NULL,
        [ProductId] [smallint]  NOT NULL,
        [Quantity] [tinyint]  NOT NULL,
        [Price] [decimal](9,2)  NOT NULL,
        [TotalAmount] [decimal](9,2)  NOT NULL,
        [TransactionDateId] [int]  NOT NULL,
        [ProfitAmount] [decimal](9,2)  NOT NULL,
        [Hour] [tinyint]  NOT NULL,
        [Minute] [tinyint]  NOT NULL,
        [StoreId] [smallint]  NOT NULL
      )
      WITH
      (
        DISTRIBUTION = HASH ( [CustomerId] ),
        CLUSTERED COLUMNSTORE INDEX,
        PARTITION
        (
          [TransactionDateId] RANGE RIGHT FOR VALUES (
            20180101, 20180201, 20180301, 20180401, 20180501, 20180601, 20180701, 20180801, 20180901, 20181001, 20181101, 20181201,
            20190101, 20190201, 20190301, 20190401, 20190501, 20190601, 20190701, 20190801, 20190901, 20191001, 20191101, 20191201)
        )
      );
    ```

4. From the top toolbar, select the **Discard all** button as we will not be saving this query. When prompted, choose to **Discard all changes**.

   ![The top toolbar menu is displayed with the Discard all button highlighted.](media/toptoolbar_discardall.png)
  
### Task 6: Populate the sale table

The data that we will be retrieving to populate the sale table is currently stored as a series of parquet files in the **solliancepublicdata** public blob storage account. This storage account has already been added as a linked service in Azure Synapse Analytics when the environment was provisioned. The sale data for each day is stored in a separate parquet file which is placed in storage following a known convention. In this lab, we are interested in populating the Sale table with only 2018 and 2019 data.

> **Note**: The current folder structure for daily sales data is as follows: /wwi-02/sale-small/Year=`YYYY`/Quarter=`Q#`/Month=`M`/Day=`YYYYMMDD` - where `YYYY` is the 4 digit year (eg. 2019), `Q#` represents the quarter (eg. Q1), `M` represents the numerical month (eg. 1 for January) and finally `YYYYMMDD` represents a numeric date format representation (eg. `20190516` for May 16, 2019).
> A single parquet file is stored each day folder with the name **sale-small-YYYYMMDD-snappy.parquet** (replacing `YYYYMMDD` with the numeric date representation).

```text
  Sample path to the parquet folder for January 1st, 2019:
  /wwi-02/sale-small/Year=2019/Quarter=Q1/Month=1/Day=20190101/sale-small-20190101-snappy.parquet
```

1. Similar to how we've done it before, create a new Dataset by selecting **Data** from the left menu, expanding the **+** button on the Data blade and selecting **Dataset**. We will be creating a dataset that will point to the root folder of the sales data in the public storage account.

2. In the **New dataset** blade, with the **All** tab selected, choose the **Azure Blob Storage** item. Select **Continue**.

3. In the **Select format** screen, choose the **Parquet** item. Select **Continue**.

    ![In the Select format screen, the Parquet item is highlighted.](media/dataset_format_parquet.png)

4. In the **Set properties** blade, populate the form as follows then select **OK**.
  
   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_sales_parquet** |
   | Linked service | Select **solliancepublicdata** |
   | File path - Container | Enter **wwi-02** |  
   | File path - Folder | Enter **sale-small** |
   | Import schema | Select **From connection/store** |

    ![The Set properties blade is displayed with fields populated with the values from the preceding table.](media/dataset_salesparquet_propertiesform.png)

5. Now we will need to define the destination dataset for our data. In this case we will be storing sale data in our SQL Pool. Create a new dataset by expanding the **+** button on the **Data** blade and selecting **Dataset**.

6. On the **New dataset** blade, with the **Azure** tab selected, enter **synapse** as a search term and select the **Azure Synapse Analytics (formerly SQL DW)** item. Select **Continue**.
  
7. On the **Set properties** blade, set the field values to the following, then select **OK**.

   | Field | Value |
   |-------|-------|
   | Name  | Enter **asamcw_sale_asa** |
   | Linked service | Select `SQLPool01`. |
   | Table name | Select **wwi_mcw.SaleSmall**. |  
   | Import schema | Select **From connection/store** |

    ![The Set properties blade is populated with the values specified in the preceding table.](media/dataset_saleasaform.png)
  
8. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to deploy the changes to the workspace.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

9. Since we want to filter on multiple sale year folders (Year=2018 and Year=2019) and copy only the 2018 and 2019 sales data, we will need to create a data flow to define the specific data that we wish to retrieve from our source dataset. A data flow allows you to graphically define dataset filters and transformations without writing code. These data flows can be leveraged as an activity in an orchestration pipeline. Create a new data flow, start by selecting **Develop** from the left menu, and in the **Develop** blade, expand the **+** button and select **Data flow**.

    ![From the left menu, the Develop item is selected. From the Develop blade the + button is expanded with the Data flow item highlighted.](media/develop_newdataflow_menu.png)

10. In the lower pane on the **General** tab, name the data flow by entering **ASAMCW - Exercise 2 - 2018 and 2019 Sales** in the **Name** field.

    ![The General tab is displayed with ASAMCW - Exercise 2 - 2018 and 2019 Sales entered as the name of the data flow.](media/dataflow_generaltab_name.png)

11. In the data flow designer window, select the **Add Source** box.

    ![The Add source box is highlighted in the data flow designer window.](media/dataflow_addsourcebox.png)

12. With the added source selected in the designer, in the lower pane with the **Source settings** tab selected, set the **Output stream name** to **salesdata** and for the **Dataset**, select **asamcw_sales_parquet**.
  
    ![The Source settings tab is selected displaying the Output stream name set to salesdata and the selected dataset being asamcw_sales_parquet.](media/dataflow_source_sourcesettings.png)

13. Select the **Source options** tab, and add the following as **Wildcard paths**, this will ensure that we only pull data from the parquet files for the sales years of 2018 and 2019:

    1. wwi-02/sale-small/Year=2018/\*/\*/\*/\*

    2. wwi-02/sale-small/Year=2019/\*/\*/\*/\*

      ![The Source options tab is selected with the above wildcard paths highlighted.](media/dataflow_source_sourceoptions.png)

14. At the bottom right of the **salesdata** source, expand the **+** button and select the **Sink** item located in the **Destination** section of the menu.

      ![The + button is highlighted toward the bottom right of the source element on the data flow designer.](media/dataflow_source_additem.png)

15. In the designer, select the newly added **Sink** element and in the bottom pane with the **Sink** tab selected, fill the form as follows:

    | Field | Value |
    |-------|-------|
    | Output stream name  | Enter **sale** |
    | Incoming stream | Select **salesdata**. |
    | Dataset | Select **asamcw_sale_asa**. |

    ![The Sink tab is displayed with the form populated with the values from the preceding table.](media/dataflow_sink_sinktab.png)

16. Select the **Mapping** tab and toggle the **Auto mapping** setting to the off position. You will need to add the following input to output column mappings. Add a mapping by selecting the **+ Add mapping** button.
  
    | Input column | Output column |
    |-------|-------|
    | TransactionDate  | TransactionDateId |
    | Hour | Hour |
    | Minute | Minute |
    | Quantity | Quantity |

    ![The Mapping tab is selected with the Auto mapping toggle set to the off position. The + Add mapping button is highlighted along with the mapping entries specified in the preceding table.](media/dataflow_sink_mapping.png)

17. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to deploy the new data flow to the workspace.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

18. We can now use this data flow as an activity in a pipeline. Create a new pipeline by selecting **Orchestrate** from the left menu, and in the **Orchestrate** blade, expand the **+** button and select **Pipeline**.

19. In the bottom pane, with the **General** tab selected, enter **ASAMCW - Exercise 2 - Copy Sale Data** as the Name of the pipeline.

20. From the **Activities** menu, expand the **Move & transform** section and drag an instance of **Data flow** to the design surface of the pipeline.
  
    ![The Activities menu of the pipeline is displayed with the Move and transform section expanded. An arrow indicating a drag operation shows adding a Data flow activity to the design surface of the pipeline.](media/pipeline_sales_dataflowactivitymenu.png)

21. With the newly added data flow activity selected in the designer, in the bottom pane with the **General** tab selected, Name the data flow **ASAMCW - Exercise 2 - 2018 and 2019 Sales**.

22. Select the **Settings** tab and set the form fields to the following values:

    | Field | Value |
    |-------|-------|
    | Data flow  | Select **ASAMCW - Exercise 2 - 2018 and 2019 Sales** |
    | Staging linked service | Select `PrimaryStorage`. |
    | Staging storage folder - Container | Enter **staging** |
    | Staging storage folder - Folder | Enter **mcwsales** |

    ![The data flow activity Settings tab is displayed with the fields specified in the preceding table highlighted.](media/pipeline_sales_dataflowsettings.png)

23. In the top toolbar, select **Publish all** to publish the new dataset definitions. When prompted, select the **Publish** button to commit the changes.

    ![The top toolbar is displayed with the Publish all button highlighted.](media/publishall_toolbarmenu.png)

24. Once published, expand the **Add trigger** item on the pipeline designer toolbar, and select **Trigger now**. In the **Pipeline run** blade, select **OK** to proceed with the latest published configuration. You will see notification toast windows indicating the pipeline is running and when it has completed.

25. View the status of the pipeline run by locating the **ASAMCW - Exercise 2 - Copy Sale Data** pipeline in the Orchestrate blade. Expand the actions menu, and select the **Monitor** item.

    ![In the Orchestrate blade, the Action menu is displayed with the Monitor item selected on the ASAMCW - Exercise 2 - Copy Sale Data pipeline.](media/orchestrate_pipeline_monitor_copysaledata.png)
  
26. You should see a run of the pipeline we created in the **Pipeline runs** table showing as in progress. It will take approximately 45 minutes for this pipeline operation to complete. You will need to refresh this table from time to time to see updated progress. Once it has completed. You should see the pipeline run displayed with a Status of **Succeeded**.
  
    ![On the pipeline runs screen, a successful pipeline run is highlighted in the table.](media/pipeline_run_sales_successful.png)

27. Verify the table has populated by creating a new query. Select the **Develop** item from the left menu, and in the **Develop** blade, expand the **+** button, and select **SQL script**. In the query window, be sure to connect to the SQL Pool database (`SQLPool01`), then paste and run the following query. When complete, select the **Discard all** button from the top toolbar.

  ```sql
    select count(TransactionId) from wwi_mcw.SaleSmall;
  ```

## Exercise 5: Security

It is important to identify data columns of that hold sensitive information. Types of sensitive information could be social security numbers, email addresses, credit card numbers, financial totals, and more. Azure Synapse Analytics allows you define permissions that prevent users or roles select privileges on specific columns.

### Task 1: Column level security

  ```sql
      /*  Column-level security feature in Azure Synapse simplifies the design and coding of security in applications.
        It ensures column level security by restricting column access to protect sensitive data. */

    /* Scenario: In this scenario we will be working with two users. The first one is the CEO, he has access to all
        data. The second one is DataAnalystMiami, this user doesn't have access to the confidential Revenue column
        in the CampaignAnalytics table. Follow this lab, one step at a time to see how Column-level security removes access to the
        Revenue column to DataAnalystMiami */

    --Step 1: Let us see how this feature in Azure Synapse works. Before that let us have a look at the Campaign Analytics table.
    select  Top 100 * from wwi_mcw.CampaignAnalytics
    where City is not null and state is not null

    /*  Consider a scenario where there are two users.
        A CEO, who is an authorized  personnel with access to all the information in the database
        and a Data Analyst, to whom only required information should be presented.*/

    -- Step:2 Verify the existence of the “CEO” and “DataAnalystMiami” users in the Datawarehouse.
    SELECT Name as [User1] FROM sys.sysusers WHERE name = N'CEO';
    SELECT Name as [User2] FROM sys.sysusers WHERE name = N'DataAnalystMiami';


    -- Step:3 Now let us enforcing column level security for the DataAnalystMiami.
    /*  The CampaignAnalytics table in the warehouse has information like ProductID, Analyst, CampaignName, Quantity, Region, State, City, RevenueTarget and Revenue.
        The Revenue generated from every campaign is classified and should be hidden from DataAnalystMiami.
    */

    REVOKE SELECT ON wwi_mcw.CampaignAnalytics FROM DataAnalystMiami;
    GRANT SELECT ON wwi_mcw.CampaignAnalytics([ProductID], [Analyst], [Product], [CampaignName],[Quantity], [Region], [State], [City], [RevenueTarget]) TO DataAnalystMiami;
    -- This provides DataAnalystMiami access to all the columns of the Sale table but Revenue.

    -- Step:4 Then, to check if the security has been enforced, we execute the following query with current User As 'DataAnalystMiami', this will result in an error
    --  since DataAnalystMiami doesn't have select access to the Revenue column
    EXECUTE AS USER ='DataAnalystMiami';
    select TOP 100 * from wwi_mcw.CampaignAnalytics;
    ---
    -- The following query will succeed since we are not including the Revenue column in the query.
    EXECUTE AS USER ='DataAnalystMiami';
    select [ProductID], [Analyst], [Product], [CampaignName],[Quantity], [Region], [State], [City], [RevenueTarget] from wwi_mcw.CampaignAnalytics;

    -- Step:5 Whereas, the CEO of the company should be authorized with all the information present in the warehouse.To do so, we execute the following query.
    Revert;
    GRANT SELECT ON wwi_mcw.CampaignAnalytics TO CEO;  --Full access to all columns.

    -- Step:6 Let us check if our CEO user can see all the information that is present. Assign Current User As 'CEO' and the execute the query
    EXECUTE AS USER ='CEO'
    select * from wwi_mcw.CampaignAnalytics
    Revert;
  ```

### Task 2: Row level security

  ```sql
    /* Row level Security (RLS) in Azure Synapse enables us to use group membership to control access to rows in a table.
    Azure Synapse applies the access restriction every time the data access is attempted from any user. 
    Let see how we can implement row level security in Azure Synapse.*/

  ----------------------------------Row-Level Security (RLS), 1: Filter predicates------------------------------------------------------------------
  -- Step:1 The Sale table has two Analyst values: DataAnalystMiami and DataAnalystSanDiego. 
  --     Each analyst has jurisdiction across a specific Region. DataAnalystMiami on the South East Region
  --      and DataAnalystSanDiego on the Far West region.
  SELECT DISTINCT Analyst, Region FROM wwi_mcw.CampaignAnalytics order by Analyst ;

  /* Scenario: WWI requires that an Analyst only see the data for their own data from their own region. The CEO should see ALL data.
      In the Sale table, there is an Analyst column that we can use to filter data to a specific Analyst value. */

  /* We will define this filter using what is called a Security Predicate. This is an inline table-valued function that allows
      us to evaluate additional logic, in this case determining if the Analyst executing the query is the same as the Analyst
      specified in the Analyst column in the row. The function returns 1 (will return the row) when a row in the Analyst column is the same as the user executing the query (@Analyst = USER_NAME()) or if the user executing the query is the CEO user (USER_NAME() = 'CEO')
      whom has access to all data.
  */

  -- Review any existing security predicates in the database
  SELECT * FROM sys.security_predicates

  --Step:2 Create a new Schema to hold the security predicate, then define the predicate function. It returns 1 (or True) when
  --  a row should be returned in the parent query.
  GO
  CREATE SCHEMA Security
  GO
  CREATE FUNCTION Security.fn_securitypredicate(@Analyst AS sysname)  
      RETURNS TABLE  
  WITH SCHEMABINDING  
  AS  
      RETURN SELECT 1 AS fn_securitypredicate_result
      WHERE @Analyst = USER_NAME() OR USER_NAME() = 'CEO'
  GO
  -- Now we define security policy that adds the filter predicate to the Sale table. This will filter rows based on their login name.
  CREATE SECURITY POLICY SalesFilter  
  ADD FILTER PREDICATE Security.fn_securitypredicate(Analyst)
  ON wwi_mcw.CampaignAnalytics
  WITH (STATE = ON);

  ------ Allow SELECT permissions to the Sale Table.------
  GRANT SELECT ON wwi_mcw.CampaignAnalytics TO CEO, DataAnalystMiami, DataAnalystSanDiego;

  -- Step:3 Let us now test the filtering predicate, by selecting data from the Sale table as 'DataAnalystMiami' user.
  EXECUTE AS USER = 'DataAnalystMiami'
  SELECT * FROM wwi_mcw.CampaignAnalytics;
  revert;
  -- As we can see, the query has returned rows here Login name is DataAnalystMiami

  -- Step:4 Let us test the same for  'DataAnalystSanDiego' user.
  EXECUTE AS USER = 'DataAnalystSanDiego';
  SELECT * FROM wwi_mcw.CampaignAnalytics;
  revert;
  -- RLS is working indeed.

  -- Step:5 The CEO should be able to see all rows in the table.
  EXECUTE AS USER = 'CEO';  
  SELECT * FROM wwi_mcw.CampaignAnalytics;
  revert;
  -- And he can.

  --Step:6 To disable the security policy we just created above, we execute the following.
  ALTER SECURITY POLICY SalesFilter  
  WITH (STATE = OFF);

  DROP SECURITY POLICY SalesFilter;
  DROP FUNCTION Security.fn_securitypredicate;
  DROP SCHEMA Security;
  ```

### Task 3: Dynamic data masking

```sql
    -------------------------------------------------------------------------Dynamic Data Masking (DDM)----------------------------------------------------------------------------------------------------------
    /*  Dynamic data masking helps prevent unauthorized access to sensitive data by enabling customers
        to designate how much of the sensitive data to reveal with minimal impact on the application layer.
        Let see how */

    /* Scenario: WWI has identified sensitive information in the CustomerInfo table. They would like us to
        obfuscate the CreditCard and Email columns of the CustomerInfo table to DataAnalysts */

    -- Step:1 Let us first get a view of CustomerInfo table.
    SELECT TOP (100) * FROM wwi_mcw.CustomerInfo;

    -- Step:2 Let's confirm that there are no Dynamic Data Masking (DDM) applied on columns.
    SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
    FROM sys.masked_columns AS c  
    JOIN sys.tables AS tbl
        ON c.[object_id] = tbl.[object_id]  
    WHERE is_masked = 1
        AND tbl.name = 'CustomerInfo';
    -- No results returned verify that no data masking has been done yet.

    -- Step:3 Now lets mask 'CreditCard' and 'Email' Column of 'CustomerInfo' table.
    ALTER TABLE wwi_mcw.CustomerInfo  
    ALTER COLUMN [CreditCard] ADD MASKED WITH (FUNCTION = 'partial(0,"XXXX-XXXX-XXXX-",4)');
    GO
    ALTER TABLE wwi_mcw.CustomerInfo
    ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()');
    GO
    -- The columns are successfully masked.

    -- Step:4 Let's see Dynamic Data Masking (DDM) applied on the two columns.
    SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
    FROM sys.masked_columns AS c  
    JOIN sys.tables AS tbl
        ON c.[object_id] = tbl.[object_id]  
    WHERE is_masked = 1
        AND tbl.name ='CustomerInfo';

    -- Step:5 Now, let us grant SELECT permission to 'DataAnalystMiami' on the 'CustomerInfo' table.
   GRANT SELECT ON wwi_mcw.CustomerInfo TO DataAnalystMiami;  

    -- Step:6 Logged in as  'DataAnalystMiami' let us execute the select query and view the result.
    EXECUTE AS USER = 'DataAnalystMiami';  
    SELECT * FROM wwi_mcw.CustomerInfo;

    -- Step:7 Let us remove the data masking using UNMASK permission
    GRANT UNMASK TO DataAnalystMiami;
    EXECUTE AS USER = 'DataAnalystMiami';  
    SELECT *
    FROM wwi_mcw.CustomerInfo;
    revert;
    REVOKE UNMASK TO DataAnalystMiami;  

    ----step:8 Reverting all the changes back to as it was.
    ALTER TABLE wwi_mcw.CustomerInfo
    ALTER COLUMN CreditCard DROP MASKED;
    GO
    ALTER TABLE wwi_mcw.CustomerInfo
    ALTER COLUMN Email DROP MASKED;
    GO
```

## Exercise 7: Monitoring

### Task 1: Workload Importance

### Task 2: Workload Isolation

### Task 3: Monitoring with Dynamic Management Views

### Task 4: Orchestration Monitoring with the Monitor Hub

### Task 5: Monitoring SQL Requests with the Monitor Hub

## After the hands-on lab

Duration: X minutes

You should follow all steps provided *after* attending the Hands-on lab.
