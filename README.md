# Databricks Starter Project

This is a starter project for anyone who want to get started using Azure Databricks.  It will guide you through some activities that help you build your
own [lakehouse](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/).  This should familiarize you with basic concepts of the lakehouse and
give you some practice using tools in Azure.

For this project, we will be using the [NYC Taxi data](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page). This is an open dataset provided
by the New York City Taxi & Limousine.  It contains a record of every taxi and ride-sharing ride taken in New York city since 2009.  We often use this
dataset in training activities because it has several nice features:
   1. it's free
   2. it's easy to understand because we all know what a taxi ride is
   3. it's real-world data so it's not perfectly clean or standardized
   4. it's a large dataset so we get experience working with many rows of data

This project guides you through the creation of a simple version of the lakehouse architecture.  This includes creating bronze, silver,
and gold layers so you will want to be sure you are familiar with the
[Databricks medallion architecture](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion) to understand what these terms mean.

You will copy the raw data into your environment and then use Databricks to apply several rounds of data cleaning and enrichment.
This will give you practical experience creating multiple datasets in a lakehouse environment and demonstrating how the data progresses
through the various layers of preparation.

## Step 0 - Set Up Your Environment

We'll begin by preparing an Azure environment in which you can complete this project.  Note that you will be creating two Azure resources
(an Azure Databricks workspace and a Storage Account).  When creating a environment in Azure, it is best practice to create all of the resources
for that environment in the *same region*.  For this project, I would prefer that you select East US 2.

1. Provision an Azure subscription (if you do not already have one)
1. Create a new resource group for this project.  You may name it whatever you want, but something like `rg_databricks_starter` will help you keep track of it.
1. Create a new Azure Databricks workspace in your resource group.
1. Create a new ADLS Gen 2 storage account in your resource group. (***NOTE:*** Be _sure_ that you [create an ADLS Gen 2 storage account](https://learn.microsoft.com/en-us/azure/storage/blobs/create-data-lake-storage-account) and not just a plain blob storage account.
   1. Add a new container to hold your raw data.
   1. Add a second container to hold your bronze data.
1. Create a git repo where you can store your work.  This will allow you to share your work with me and give you practice working in a source-controlled Databricks environment.
   1. If you prefer to use GitHub, you can create a new repo there.
   1. *or* if you prefer to use Azure DevOps, you can create a repo in that service instead.
1. In Databricks, configure your git credentials and then connect to your repo.  All of the notebooks you create in this project should be created in the repo.
1. Mount the two containers from your ADLS Gen 2 storage account on the DBFS.

#### Discussion Questions for Step 0
1. Why is it important to create the resources in the same region?  Does this affect cost, performance, or both?
1. Why did we put both of our resources in the same resource group?
1. When you mounted your storage account on the DBFS, which authentication method did you use?  What are the advantages and disadvantages
of this method compared to other methods?

## Step 1 - Get the Raw Data

I will provide you with a SAS key and account name so you can copy this data.  You do *not* need to write a script to scrape the data from the NYC Taxi website.

In this step, you will need to copy the raw data files from my storage account to the raw container in your storage account.
This will give you experience with moving large volumes of data in Azure.  It will also make your project self-contained and
not reliant on my Azure subscription.

There are many different ways that you can copy the data.  Feel free to select any method that you want.  Options include:
 - using the Azure Storage Explorer application
 - using the `azcopy` command line utility
 - writing Spark code in Databricks to iterate over the files and copy them

(*Hint*:  Using Azure Storage Explorer will be the easiest and fastest way to do it.)

#### Discussion Questions for Step 1
1. What tools did you use to copy the data?  Why did you choose those tools?
1. How much data did you copy?
1. How much does it cost you to store that volume of data?
1. For this exercise, you performed a one-time move of the data.  In the real world, these processes would be automated so fresh, raw data
can be continually ingested.  What are some tools you might use for this?

## Step 2 - Ingest Raw Data to Bronze Layer

Now that you have the raw data stored in your lakehouse, you will want to begin the process of cleaning it so that it can be used more easily
for analysis.  For our first step, we will create the Bronze Layer of our lakehouse.  In this step, we will simply transform the data
from individual parquet files into a single [Delta Lake](https://learn.microsoft.com/en-us/azure/databricks/delta/) table.  We will not need
to apply any additional cleansing at this time.  We are simply copying the data from parquet to Delta.

When you write the data to your bronze layer, you will want to partition the data.  Read about big data partitioning and think about which column(s)
you will want to use to partition the data.

The raw data that you copied is in parquet files.  However, these files were created at different points in time.  They do not all have
the same schema.  (That is to say that the schema for the data evolves over time.) 

Since the raw data has a variety of schemas, the files must be read in one at a time.  This requires some slightly advanced coding.  Therefore, I
have written the following script to help you.  Paste this code into a notebook cell in Databricks:
```python
path_to_parquet = "/mnt/taxi/parquet-raw"

import pyspark.sql.functions as F
df = None
for type in dbutils.fs.ls(path_to_parquet):
  for file in dbutils.fs.ls(type[0]):
    file_df = spark.read.parquet(file[0]).withColumn("taxi_type", F.lit(type[1][:-1])).withColumn("source_file", F.input_file_name())
    if df is None:
      df = file_df
    else:
      df = df.unionByName(file_df, allowMissingColumns=True)
      
print(df)
```

This code will take 5 &mdash; 10 minutes to run.  When it completes, you will have a Spark dataframe referenced in a variable called `df`.
You can then apply partitioning to the dataframe and write it to your Bronze Layer as a Delta table.  (There are plenty of examples
online that show how you can do this.)

#### Discussion Questions for Step 2
1. What column(s) did you choose to partition on?  What factors did you consider in making your choice?
1. How long did it take to write the Delta files?  What could you do to make this process faster?
1. Review the names of the columns in the dataframe.  Do you see anything that will need to be cleaned up later in the Silver Layer?


## Step 3 - Clean Data in Bronze Layer and Write to Silver Layer

*detailed steps to be written*

## Step 4 - Create Analysis-Ready Datasets in Gold Layer

*detailed steps to be written*

## Step 5 - Prepare an Analysis of the Data

*detailed steps to be written*

## Step 6 - (Optional) Serve Lakehouse Data From Synapse

*maybe???*
