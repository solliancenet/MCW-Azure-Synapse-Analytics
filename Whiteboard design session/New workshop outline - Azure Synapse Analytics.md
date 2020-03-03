![Microsoft Cloud Workshop](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
[Insert workshop name here]
</div>

<div class="MCWHeader2">
New workshop outline
</div>

<div class="MCWHeader3">
[Insert date here Month Year]
</div>

## \[Insert workshop title here\] outline

  **Team**                        **Name**                            **Email**
  ------------------------------- ----------------------------------- ------------------------------------
  Program manager                 Insert the program manager's name   Insert the program manager's email
  Project manager                 Insert the project manager's name   Insert the project manager's name
  Tech lead                       Insert tech lead's name             Insert tech lead's email
  Content developer vendor name   Insert vendor's name                Insert vendor's email
  Stakeholder                     Insert stakeholders' name(s)        Insert stakeholders' email
  SME Team                        Insert lead SME name                Insert lead SME email

  **Workshop details**      **Description**
  ------------------------- --------------------------------------------------------------------------------------
  Kickoff date              Insert the project kick-off date
  Release date              Insert the project release date
  Target audience           
  Hero solution             Insert Hero solution
  Technical use case        Insert the technical use case
  Products/Services         Insert the products/services covered in workshop
  Link to source material   Insert the link(s) to any source material used to create the workshop, if applicable

## Abstract 

Wide World Importers (a fictitious retail customer) has hundreds of brick and mortar stores and an online store where they sell all kinds of products. They would like to:

* Gain business insights using historical, real-time, and predictive analytics using structured and unstructured data sources
*	Enable their IT Team of data engineers and data scientists to bring in and run complex queries over petabytes of structured data with billions of rows and unstructured enterprise operational data
*	Enable business analysts and IT team to share a single source of truth and have a single workspace to collaborate and work with enterprise data and enriched customer data
*	They would like to minimize the number of disparate services they use across ingest, transformation, querying and storage, so that their team of data engineers, data scientists and database administrators can master one tool, and can build shared best practices for development, management and monitoring.


### Workshop

### Whiteboard design session *(this will go in the readme and in the WDS document)*

High Level Architecture
*	Diagram your initial vision for handling the top-level requirements for data loading, data transformation, storage, machine learning modeling, and reporting.

Ingest & Store
*	WWI does not want to have to rely on specialists to create data ingest and data transformation pipelines. They would like to split the problem into two efforts: 
    * land data from new data sources in their data lake
    * preparing, merging and transforming data for querying from the data warehouse
They would like to accomplish both using tools that simplify the building of these transformation pipelines using a graphical designer, only dropping down into code when absolutely necessary.
*	For the solution you recommend, what specific approach is the most performant for moving flat file data from the ingest storage locations to the data lake? What file formats would WWI need to ensure the source data provides in order to use this option?

    * Follow pattern of landing in lake first, then ingest into warehouse. Extract using ADF, as Parquet files.
*	What storage service would you recommend they use and how would you recommend they structure the folders so they can manage the data at the various levels of refinement?
    * ADLS gen 2; example structure: bronze, silver, gold, platinum containers.
*	When it comes to ingesting raw data in batch from new data sources, what data formats would they support?
    * CSV, Parquet, ORC, JSON  
*	How will you ingest streaming data from the in-store IoT devices?
    *   Collect in Event Hub or IoT Hub, process with Stream Analytics (Confirm this is recommendation for April timeframe).

Transform
*	Before building transformation pipelines or loading it into the data warehouse, how can WWI quickly explore the raw ingested data to understand its contents?
    *   Using Studio, right click to query a SQL or as DataFrame in a notebook.
*	When it comes to storing refined versions of the data for possible querying, what data format would you recommend they use? Why?
    *   Parquet. There is industry alignment around Parquet format for sharing (e.g., across Hadoop, Databricks, and SQL engine scenarios) data at the storage layer. In addition to Azure Synapse Analytics, Parquet is supported by other services, such as Power BI. Parquet is a perfomant, column oriented format optimized for big data scenarios. 
*	Regarding the service you recommend they use for preparing, merging and transforming the data, in which situations can you use the graphical designer and which situations would require code? 
    * The could use Mapping Data Flows that they graphically design in Studio. These code-free data flows provide for scalable execution which builds DSL for transformation, into code that runs on Spark, which runs at scale and provides elasticity for handling growing volumes of data. 
    * They can use code when their data engineers want to use Spark to transform the data via DataFrames. 
*	Their data team is accustomed to leveraging open source packages that help them quickly pre-process the data, as well as enable their data scientists to train machine learning models using both Spark and python. Explain how your solution would enable this.
    *  Synapse support open source Apache Spark and the execution of Python, Scala and (in the near future) R code. Their data team would be able to use the familiar Jupyter notebooks and leverage their favorite libraries.
*	Does your solution allow their data engineers and data scientists to work within Jupyter notebooks? How are libraries managed?
    *  Apache Spark pools allow the importing of libraries during creation.
    *  These dependencies are specified using a PIP freeze formatted document listing the desired library name and version.
    *  The data team can then launch notebooks attached to the Spark pool and author the code that uses their favorite libraries.  


Query
*	Their retail transaction dataset exceeds a billion rows. For their downstream reporting queries, they need to be able to join, project and filter these rows in no longer than 10s of seconds. WWI is concerned their data is just to big to do this. 
    * What specific indexing techniques should they use to reach this kind of performance for their fact tables? Why?
      * Clustered Columnstore Indexes. As they offer the highest level of data compression and best overall query performance, columnstore indexes are usually the best choice for large tables such as fact tables. 
    * Would you recommend the same approach for tables they have with less than 100 million rows?
      * No. For "small" tables with less than 100 million rows, they should consider Heap tables.
    * How should they configure indexes on their smaller lookup tables?
      * The should consider using Heap tables. For small lookup tables, less than 100 million rows, often heap tables make sense. Cluster columnstore tables begin to achieve optimal compression once there is more than 100 million rows.
    * What would you suggest for their larger lookup tables that are used just for point lookups that retrieve only a single row? How could they makes these more flexible so that queries filtering against different sets of columns would still yield performant lookups?
      * Use clustered indexes. Clustered indexes may outperform clustered columnstore tables when a single row needs to be quickly retrieved. For queries where a single or very few row lookup is required to performance with extreme speed, consider a cluster index or nonclustered secondary index. The disadvantage to using a clustered index is that only queries that benefit are the ones that use a highly selective filter on the clustered index column. To improve filter on other columns a nonclustered index can be added to other columns. However, each index which is added to a table adds both space and processing time to loads.

    * What should they use for the fastest loading of staging tables?
      * A heap table. If you are loading data only to stage it before running more transformations, loading the table to heap table is much faster than loading the data to a clustered columnstore table. 
      * In addition, loading data to a temporary table loads faster than loading a table to permanent storage.
*	What are the typical issues they should look out for with regards to **distributed** table design for the following scenarios? 
    * Their smallest fact table exceeds several GB’s and by their nature experiences frequent inserts. 
      * They should use a hash distribution. 
      * A hash-distributed table distributes table rows across the Compute nodes by using a deterministic hash function to assign each row to one distribution.
      * Since identical values always hash to the same distribution, the data warehouse has built-in knowledge of the row locations. SQL Data Warehouse uses this knowledge to minimize data movement during queries, which improves query performance.
      * Hash-distributed tables work well for large fact tables in a star schema. They can have very large numbers of rows and still achieve high performance. 
      * Consider using a hash-distributed table when:
        - The table size on disk is more than 2 GB.
        - The table has frequent insert, update, and delete operations.
    * As they develop the data warehouse, the WWI data team identified some tables created from the raw input that might be useful, but they don’t currently join to other tables and they are not sure of the best columns they should use for distributing the data. 
      * They should consider round-robin distribution.
      * A round-robin distributed table distributes table rows evenly across all distributions. The assignment of rows to distributions is random. Unlike hash-distributed tables, rows with equal values are not guaranteed to be assigned to the same distribution.
      * As a result, the system sometimes needs to invoke a data movement operation to better organize your data before it can resolve a query. This extra step can slow down your queries. For example, joining a round-robin table usually requires reshuffling the rows, which is a performance hit.
      * Consider using the round-robin distribution for your table in the following scenarios:
            
          * When getting started as a simple starting point since  it is the default
          * If there is no obvious joining key
          * If there is not good candidate column for hash distributing the table
          * If the table does not share a common join key with other tables
          * If the join is less significant than other joins in the query
            When the table is a temporary staging table

    * They sometimes use temporary staging tables in their data preparation. 
      * The should use a round-robin distibuted table.
    * They have lookup tables that range from several hundred MBs to 1.5 GBs.
      * The should consider using replicated tables.
      * A replicated table has a full copy of the table accessible on each Compute node. Replicating a table removes the need to transfer data among Compute nodes before a join or aggregation. Since the table has multiple copies, replicated tables work best when the table size is less than 2 GB compressed.

*	Some of their data contains columns in the JSON format, how could they flatten these hierarchical fields to a tabular structure? 
    * The can use SQL on-demand along with the T-SQL OPENJSON, JSON_VALUE, and JSON_QUERY statements.
*	What approach can they use to update the JSON data?
    * The can use JSON_MODIFY with the UPDATE statement. 
*	In some of their queries, they are OK trading off speed of returning counts for a small reduction in accuracy. How might they do this?
  * They should use the APPROXIMATE_COUNT_DISTINCT statement which uses the HyperLogLog to return a result with an average 2% accuracy of the true cardinality. For example, if COUNT(DISTINCT) returns 1,000,000, then with approximate execution you will get a value between 999,736 to 1,016,234.
*	Many of their downstream reports are used by many users, which often means the same query is being executed repeatedly against data that does not change that often. What can WWI to improve the performance of these types of queries? How does this approach work when the underlying data changes?
    * They should consider result-set caching. 
    * Cache the results of a query in SQL pool storage. This enables interactive response times for repetitive queries against tables with infrequent data changes.
    * The result-set cache persists even if SQL pool is paused and resumed later. 
    * Query cache is invalidated and refreshed when underlying table data or query code changes.
    * Result cache is evicted regularly based on a time-aware least recently used algorithm (TLRU). 

*	They have heard of serverless querying, does your solution offer this? Does it support querying the data at the scale of WWI and what formats does it support? Would this be appropriate for supporting their dashboards or reports?
    * Azure Synapse Analytics support serverless querying via SQL on-demand. 
    * SQL On-Demand is an interactive query service that provides T-SQL queries over high scale data in Azure Storage.
    * Supports data in various formats (Parquet, CSV, JSON)
    * It would be appropriate for dashboards and reports, as it supports Power BI.



Visualize
*	What product can WWI use to visualize their retail transaction data? Is it a separate tool that they need to install? 
  * They can create, edit and view Power BI reports directly within the Azure Synapse Analytics workspace.
*	Can they use this same tool visualize both the batch and streaming data in a single dashboard view?
    * Yes. Power BI can be used to create dashboard that visualize both kinds of data.
*	With the product you recommend, do they need to load all the data into the data warehouse before they can create reports against it?
    * No, they only need to load the data into storage. Using SQL On-demand and Power BI they can create reports against the data directly.


Manage
*	In previous efforts, WWI systems struggled with their popularity. Exploratory queries that were not time sensitive would saturate the available resources and delay the execution of higher priority queries supporting critical reports. Explain how your solution helps to resolve this. 
    * Workload Management.
    * It manages resources, ensures highly efficient resource utilization, and maximizes return on investment (ROI). 
    * The three pillars of workload management are
      * Workload Classification – To assign a request to a workload group and setting importance levels.
    * Workload Importance – To influence the order in which a request gets access to resources.
    * Workload Isolation – To reserve resources for a workload group.

*	What does your solution provide to WWI to help them identify issues such as suboptimal table distribution, data skew, cache misses, tempdb contention and suboptimal plan selection?
  * They can leverage the Azure Advisor recommendations.
*	WWI recognizes there is a balance between the data warehouse software staying up to date and when they can afford downtime that might result. How can they establish their preferences with your solution so they are never caught off guard with an upgrade?
    * They should leverage maintenance windows. 
    * Choose a time window for your upgrades.
    * Select a primary and secondary window within a seven-day period.
    * Windows can be from 3 to 8 hours.
    * 24-hour advance notification for maintenance events.
    * Benfits include: Ensure upgrades happen on your schedule; Predictable planning for long-running jobs; Stay informed of start and end of maintenance.

*	How does your solution provide security across both SQL and Spark workloads? 
*	Can the solution help WWI discover, track and remediate security misconfigurations and detect threats? How?
*	Can WWI use this same solution to monitor for sensitive information by enabling them to discover, classify and protect and track access to such data?
*	From a network security standpoint, how is your solution secured?







## Hands-on lab *(this will go in the readme and in the HOL document)*




## Lab summary

Exercise 1: Ingest Historical Data into Data Lake

Exercise 2: Explore Historical Data

Exercise 3: Build Data Transformation and Loading Pipeline

Exercise 4: Loading Data with COPY

Exercise 5: Preparing Data using notebooks

Exercise 6: Ingest Streaming IoT Data

Exercise 6: Train and register machine learning model

Exercise 7: Optimize the data warehouse

Exercise 8: Complete the Dashboard

