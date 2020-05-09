# Environment setup

A SQL Pool named `SQLPool01` with a database name `SQLPool01`

## Raw data
* wwi-02/sales-small/Year=2010/Quarter=Q4/Month=12/Day=20101231/sale-small-20101231-snappy.parquet
* wwi-02/sales-small/Year=2018/*
* wwi-02/sales-small/Year=2019/*
* wwi-02/customer-info/customer-info.csv
* wwi-02/campaign-analytics/campaignanalytics.csv
* wwi-02/data-generators/generator-product/generator-product.csv
* From /Resources/json-data/*.* put it in wwi-02/product-json/*.*

## SQL Pool Logins
* asa.sql.workload01
* asa.sql.workload02

## Key Vault Secrets
* SQL-USER-ASA (stores the password for `asa.sql.workload01` and `asa.sql.workload02`, used by the below linked services)

## Linked Services
* sqlpool01_workload01
* sqlpool01_workload02
* solliancepublicdata
* sqlpoolXX

## Datasets
* asamcw_wwi_salesmall_workload1_asa
* asamcw_wwi_salesmall_workload2_asa
* /Resources/Datasets/asamcw_product_asa.json
* /Resources/Datasets/asamcw_product_csv.json
  
## Notebooks
* /Resources/Notebooks/ASAMCW - Exercise 6 - Machine Learning.ipynb

## Pipelines
* ASAMCW - Exercise 7 - ExecuteBusinessAnalystQueries
* ASAMCW - Exercise 7 - ExecuteDataAnalystAndCEOQueries
* /Resources/Pipelines/asamcw_exercise2_copyproductinformation.json

## Setup SQL Queries to be run upon pool creation
> Note: the user who created the environment needs to be added as a user and to the db_owner role to the SQL Pool.
- run /Resources/ASAMCW - Create SQL Users and Schema.sql