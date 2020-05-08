# Lab 8 Environment Setup

A SQL Pool named `SQLPool01` with a database name `SQLPool01`

## Tables
`wwi_mcw.SaleSmall` with an integer `CustomerId` column

## SQL Pool Logins
* asa.sql.workload01
* asa.sql.workload02

## Key Vault Secrets
* SQL-USER-ASA (stores the password for `asa.sql.workload01` and `asa.sql.workload02`, used by the below linked services)

## Linked Services
* sqlpool01_workload01
* sqlpool01_workload02

## Datasets
* asamcw_wwi_salesmall_workload1_asa
* asamcw_wwi_salesmall_workload2_asa

## Pipelines
* ASAMCW - Exercise 7 - ExecuteBusinessAnalystQueries
* ASAMCW - Exercise 7 - ExecuteDataAnalystAndCEOQueries