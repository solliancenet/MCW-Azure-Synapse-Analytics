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

* Gain business insights using hist    * rical, real-time, and predictive analytics using structured and unstructured data sources
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
*	What storage service would you recommend they use and how would you recommend they structure the folders so they can manage the data at the various levels of refinement?
*	When it comes to ingesting raw data in batch from new data sources, what data formats would they support?
*	How will you ingest streaming data from the in-store IoT devices?

Transform
*	Before building transformation pipelines or loading it into the data warehouse, how can WWI quickly explore the raw ingested data to understand its contents?
*	When it comes to storing refined versions of the data for possible querying, what data format would you recommend they use? Why?
*	Regarding the service you recommend they use for preparing, merging and transforming the data, in which situations can you use the graphical designer and which situations would require code? Give an example of the type of code they would need to write.
*	Their data team is accustomed to leveraging open source packages that help them quickly pre-process the data, as well as enable their data scientists to train machine learning models using both Spark and python. Explain how your solution would enable this. 
*	Does your solution allow their data engineers and data scientists to work within Jupyter notebooks? How are libraries managed?

Query
*	Their retail transaction dataset exceeds a billion rows. For their downstream reporting queries, they need to be able to join, project and filter these rows in no longer than 10s of seconds. WWI is concerned their data is just to big to do this. 
    * What specific indexing techniques should they use to reach this kind of performance for their fact tables? Why?
    * Would you recommend the same approach for tables they have with less than 100 million rows?
    * How should they configure indexes on their smaller lookup tables?
    * What would you suggest for their larger lookup tables that are used just for point lookups that retrieve only a single row? How could they makes these more flexible so that queries filtering against different sets of columns would still yield performant lookups?
    * What should they use for the fastest loading of staging tables?
*	What are the typical issues they should look out for with regards to distributed table design for the following scenarios? 
    * Their smallest fact table exceeds several GB’s and by their nature experiences frequent inserts. 
    * As they develop the data warehouse, the WWI data team identified some tables created from the raw input that might be useful, but they don’t currently join to other tables and they are not sure of the best columns they should use for distributing the data. 
    * They sometimes use temporary staging tables in their data preparation. 
    * They have lookup tables that range from several hundred MBs to 1.5 GBs.
*	Some of their data contains columns in the JSON format, how could they flatten these hierarchical fields to a tabular structure? 
*	Can they use a similar approach to update the JSON data?
*	In some of their queries, they are OK trading off speed of returning counts for a small reduction in accuracy. How might they do this?
*	Many of their downstream reports are used by many users, which often means the same query is being executed repeatedly against data that does not change that often. What can WWI to improve the performance of these types of queries? How does this approach work when the underlying data changes?
*	They have heard of serverless querying, does your solution offer this? Does it support querying the data at the scale of WWI and what formats does it support? Would this be appropriate for supporting their dashboards or reports?
Visualize
*	What product can WWI use to visualize their retail transaction data? Is it a separate tool that they need to install? 
*	Can they use this same tool visualize both the batch and streaming data in a single dashboard view?
*	With product you recommend, do they need to load all the data into the data warehouse before they can create reports against?
Manage
*	In previous efforts, WWI systems struggled with their popularity. Exploratory queries that were not time sensitive would saturate the available resources and delay the execution of higher priority queries supporting critical reports. Explain how your solution helps to resolve this. 
*	What does your solution provide to WWI to help them identify issues such as suboptimal table distribution, data skew, cache misses, tempdb contention and suboptimal plan selection?
*	WWI recognizes there is a balance between the data warehouse software staying up to date and when they can afford downtime that might result. How can they establish their preferences with your solution so they are never caught off guard with an upgrade?
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

