# NYC Airbnb 2019 Data ETL Pipeline using Azure Data Factory
Developed an ETL pipeline for ingestion of data from API using HTTP linked service.
Leveraged Azure Key Vault to securely store and use access tokens
Implemented dynamic parameters during ingestion activity to dynamically add time of creation
Utilized Azure Databricks and Pyspark to perform data cleaning on ingested data and load into the SILVER stage using Delta Files
Performed ad-hoc data analysis as per business requirements and onehot encoding to further stage the data into GOLD level for access by data analysts and data scientists.


# Overview

This repository documents the creation and configuration of an Azure Data Factory (ADF) pipeline integrated with Databricks for an end-to-end ETL process. It leverages Azure Storage Accounts for Medallion architecture, Databricks notebooks for transformations, and Azure Key Vault for secure integration.

# Step-by-Step Process  
Detailed Steps for Setting Up an ADF Pipeline with Databricks Integration

# Step 1: Setting Up Storage Accounts for Medallion Architecture  
1.Source Storage Account (sourceingestion98):  
oCreate a storage account named "eg:sourceingestion98".  
oInside this account, create a container (e.g., sales-data).  
oUpload your source file (e.g., sales_data.csv) to this container.  

2.Destination Storage Account (projectdatabricks98):  
oCreate another storage account named projectdatabricks98.
oAdd three containers:    
--rawdata for storing unprocessed data.  
--curated for storing processed intermediate data.  
--staging for storing the final, transformed data.  


Explanation:  
This structure follows the Medallion Architecture (Raw → Curated → Staging), which organizes data flow into layers for better processing and management.  

# Step 2: Creating and Configuring ADF  

1.Create the ADF Instance:    
oGo to the Azure Portal and create an instance of Azure Data Factory.    
oLaunch the Data Factory Studio for designing pipelines. 

2.Linked Services Configuration:    
oIn ADF Studio, navigate to Manage > Linked Services.    
oAdd linked services for:    
--Source Storage: Connect to sourceingestion98.    
--Destination Storage: Connect to projectdatabricks98.  
--Databricks: Use Azure Key Vault for secrets (explained in Step 5). 

3.Pipeline Design - Copy Data Activity:  
oAdd a Copy Data activity to copy sales_data.csv from the sourceingestion storage to the raw-data container in projectstorage997.  
oConfigure dynamic parameters for the source and sink paths, enabling reusability of the pipeline.  

Explanation:
Linked Services act as connectors, allowing ADF to interact with storage accounts, Databricks, and other resources.  
Copy Data is the initial step that moves raw data into the raw layer for further processing.  

![image](https://github.com/user-attachments/assets/8579602b-1952-47a3-8222-87f675cbec39)

# Step 3: Setting Up Databricks Notebooks

1.Prepare Databricks Workspaces:  
oCreate a Databricks workspace in Azure.  
oInside Databricks, create two notebooks:  
--Curated Notebook: For intermediate processing.  
--Staging Notebook: For final transformations.  

2.Notebook for Curated Data:  
oApply data-cleaning activities and transformations such as:    
--Handling null values using isnull().    
--Data cleansing with regexp().    
--Date conversion with to_date().     
--Used abs() to convert quantity values to positive.  
--Capitalizing column data with initcap().      
--Date conversion with to_date() and coalesce() for converting different date formats in transaction date to one format.	
--Used coulmnrenamed() to change the column name .
oWrite the transformed data back to the curated container in CSV format.  

3.Notebook for Staging Data:      
oAdditional transformations:    
--Extract year from date fields.    
--Create a new column (e.g., total_quantity).    
--Utlilized StructType() to define schema for reading the data.     
--Partition data based on product type for efficient querying.  
oWrite the final output to the staging container in Parquet format.  

Explanation:    
Each notebook processes the data to refine and enhance its usability as it moves through the Medallion Architecture.  

![image](https://github.com/user-attachments/assets/306172e1-7db3-4c67-be45-13e5765a0369)

![image](https://github.com/user-attachments/assets/1342d7e4-c4ef-43a6-a2e2-2543e8ab1ae2)

# Step 4: Pipeline Integration with Databricks

1.Databricks Linked Service:  
oUse Azure Key Vault (explained in Step 5) to securely store credentials (access token and scope details).  
oCreate a Databricks linked service in ADF referencing these secrets.  

2.Pipeline Design:  
oAdd two Databricks Notebook activities to the pipeline:  
--First for the curated stage.  
--Second for the staging stage.  
oConfigure the activities to call the respective Databricks notebooks.  

Explanation:  
This step integrates Databricks with ADF, allowing it to execute the transformations defined in the notebooks.
Add Screenshot Here for Pipeline with Databricks Integration.  

![image](https://github.com/user-attachments/assets/60538bfb-5b14-4b6c-a31a-b1d1eca7f932)  

# Step 5: Setting Up Azure Key Vault  

1.Microsoft Entra ID (AAD) Creation:  
oCreate an app using app registrations  
oCreate the secret for the app and copy the value.  
oAdd a role assignment as storage Blob contributor and attach the above app for storage account(projectdatabricks98).  

2.Key Vault Creation:  
oCreate an Azure Key Vault.  
oAdd secrets for:  
--Copy the secret value from the app and create a secret.  
--Databricks access token.  

3.Access Policies:  
oGrant access to:  
--APP to configure storage account with databricks.  
--ADF: To fetch secrets.  
--Databricks: To use these secrets.  

Explanation:  
Key Vault ensures secure and centralized management of sensitive information like credentials and tokens.  

![image](https://github.com/user-attachments/assets/5800e60f-3c54-4ef1-a66a-739576d0e0f0)  

![image](https://github.com/user-attachments/assets/b99b7540-80af-41ca-9d2e-811d7e315e2c)  

# Step 6: Implementing Dynamic Parameters

1.Create Parameters in the Pipeline:  
oAdd parameters for dynamic source and sink paths in the Copy Data activity.  
oUse these parameters in the Databricks Notebook activities for flexibility.  

2.Parameterize Linked Services:  
oUse parameterized Linked Services to allow easy changes in storage or Databricks configurations without modifying the pipeline.  

Explanation:  
Dynamic parameters make the pipeline reusable and adaptable to different datasets and configurations.  

![image](https://github.com/user-attachments/assets/9651f2ab-97b0-485c-b0ea-d371f0ff80a3)

![image](https://github.com/user-attachments/assets/5000452b-eb1a-4281-95d1-3318ff5bf797)

# Step 7: Testing and Validation  

1.Run the Pipeline:    
oTest the pipeline by triggering it with the source file (sales_data.csv).    
oVerify that the data flows through the raw, curated, and staging containers as expected.    

2.Validate Outputs:  
oCheck the curated container for processed CSV files.  
oCheck the staging container for Parquet files with applied transformations and partitions.  

![image](https://github.com/user-attachments/assets/b520d59a-cb81-4633-b38b-7039e50053d7)

![WhatsApp Image 2024-12-04 at 23 33 45_b3a97869](https://github.com/user-attachments/assets/4cc3ac96-d4ea-41cf-8084-4f45376e253f)
