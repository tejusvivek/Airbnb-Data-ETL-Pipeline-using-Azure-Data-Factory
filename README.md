# NYC Airbnb 2019 Data ETL Pipeline using Azure Data Factory
Developed an ETL pipeline for ingestion of data from API using HTTP linked service.
Leveraged Azure Key Vault to securely store and use access tokens
Implemented dynamic parameters during ingestion activity to dynamically add time of creation
Utilized Azure Databricks and Pyspark to perform data cleaning on ingested data and load into the SILVER stage using Delta Files
Performed ad-hoc data analysis as per business requirements and onehot encoding to further stage the data into GOLD level for access by data analysts and data scientists.

Add readme screenshots

# Overview

This repository documents the development of an Azure Data Factory (ADF) pipeline integrated with Databricks demonstrating an end-to-end ETL process. It leverages Azure Data Lake Storage Gen2 for implementation of Medallion architecture, Azure Key Vault for securely storing, accessing, and sharing secrets, and Databricks notebooks for data transformations and loading.

# Steps Involved 

# Step 1: Deploying Storage Accounts for Medallion Architecture  
1. Storage Account:  
 o Create a storage account named.  
 o Inside this account, create a container (bronze, silver, gold).

The containers bronze, silver, gold are used for implementing the medallion architecture whereby the raw data flows into the bronze container, then followed by the staging container for silver, and the final transformed data is loaded into the gold container.

# Step 2: Creating and Configuring ADF  

1.Create the ADF Resource:    
 o Go to the Azure Portal and create an instance of Azure Data Factory.    
 o Launch the Data Factory Studio for designing pipelines. 

2.Linked Services Configuration:    













