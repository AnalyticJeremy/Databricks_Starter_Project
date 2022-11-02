# Databricks Starter Project

This is a starter project for anyone who want to get started using Azure Databricks.  It will guide you through some activities that help you build your
own [lakehouse](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/).  This should familiarize you with basic concepts of the lakehouse and
give you some practice using tools in Azure.

For this project, we will be using the [NYC Taxi data](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page). (explain and provide rationale)

You will be building a simple version of the lakehouse architecture.  This includes creating bronze, silver, and gold layers so you will want to be sure
you are familiar with the [Databricks medallion architecture](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion) to understand what
these terms mean.

## Step 0 - Set Up Your Environment

We'll begin by preparing an Azure environment in which you can complete this project.

1. Provision an Azure subscription (if you do not already have one)
1. Create a new resource group for this project.  You may name it whatever you want, but something like `rg_databricks_starter` will help you keep track of it.
1. Create a new Azure Databricks workspace in your resource group.
1. Create a new ADLS Gen 2 storage account in your resource gorup.
   1. Add a new container to hold your raw data.
   1. Add a second container to hold your bronze data.
1. Create a git repo where you can store your work.  This will allow you to share your work with me and give you practice working in a source-controlled Databricks environment.
   1. If you prefer to use GitHub, you can create a new repo there.
   1. *or* if you prefer to use Azure DevOps, you can create a repo in that service instead.
1. In Databricks, configure your git credentials and then connect to your repo.  All of the notebooks you create in this project should be created in the repo.
1. Mount the two containers from your ADLS Gen 2 storage account on the DBFS.

## Step 1 - Get the Raw Data

I will provide you with a SAS key and account name so you can copy this data.  You do *not* need to write a script to scrape the data from the NYC Taxi website.

*detailed steps to be written*

## Step 2 - Ingest Raw Data to Bronze Layer

*detailed steps to be written*

## Step 3 - Clean Data in Bronze Layer and Write to Silver Layer

*detailed steps to be written*

## Step 4 - Create Analysis-Ready Datasets in Gold Layer

*detailed steps to be written*

## Step 5 - Prepare an Analysis of the Data

*detailed steps to be written*

## Step 6 - (Optional) Serve Lakehouse Data From Synapse

*maybe???*
