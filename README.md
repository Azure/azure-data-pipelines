**Last updated, June 24, 2020** 

## Introduction 

As organizations wrestle with the impact of COVID-19 on their operations, access to data describing the progress of the pandemic and government policies can help in making informed decisions as economies and communities start opening up gradually.  

To assist in the process, Microsoft is making various COVID-19 related datasets available in a public Azure data lake.  These datasets are accessible and can be used, subject to published terms and conditions of use, to provide up-to-date COVID-19 related information to aid in planning and decision making.  In addition, Microsoft is publishing pipeline templates that show how these datasets are being ingested and transformed.  With these templates, you can set up comparable ingestion pipelines to bring similar datasets into your own Azure environment.       

With so much widespread COVID-19 impacts in the past few months, it is expected that organizations would be gearing up for disruptions and various changes in the way they continue to conduct their operations in future during this long term pandemic recovery phase. While many data communities around the world are continuing their efforts with data to help fight against this pandemic, this blog covers a few related topics – “COVID-19 Data Lake on Azure”,  “Ingestion templates/pipelines”, “Consumption of these datasets” – and how this can help analyzing insights and help organizations make informed decisions as economies and communities start opening up gradually. 

## COVID-19 Data Lake with Open Datasets  
To help make COVID-19 data more accessible, Microsoft has created a [COVID-19 Data Lake](https://azure.microsoft.com/en-us/services/open-datasets/catalog/covid-19-data-lake/), hosted on Azure Data Lake Storage in the East US region, that contains COVID-19 related datasets from various sources, focused initially on testing and patient outcome tracking data and social distancing policy. We are making datasets available for easier access within existing Azure Analytics environments for research purposes. You can access these ready-to-use datasets and incorporate them into various use cases to understand and predict the impact of COVID-19 on your communities, businesses and markets.  These datasets are available on [Azure Open Datasets](https://azure.microsoft.com/en-us/services/open-datasets/catalog/covid-19-data-lake/) - an initiative for curated collections of publicly available datasets that are ready to use in machine learning workflows and are easy to access from various Azure services. For each dataset, modified versions are available in csv, json, json-lines, and parquet formats, as well as the raw data as ingested.  

The following datasets are currently available.  Additional datasets will be added in due course. Schema details and code samples showing data access from Azure Synapse, Azure Databricks and Azure Notebooks are provided for each dataset.  

Datasets | Description 
-------- | ----------- 
[Bing COVID-19 Data](https://azure.microsoft.com/en-us/services/open-datasets/catalog/bing-covid-19-data/) | Bing COVID-19 data includes confirmed, fatal, and recovered cases from all regions, updated daily.	
[COVID Tracking Project](https://azure.microsoft.com/en-us/services/open-datasets/catalog/covid-tracking/) | The COVID Tracking Project dataset provides the latest numbers on tests, confirmed cases, hospitalizations, and patient outcomes from every US state and territory.	
[European Centre for Disease Prevention and Control (ECDC) Covid-19 Cases](https://azure.microsoft.com/en-us/services/open-datasets/catalog/ecdc-covid-19-cases/) | The latest available public data on geographic distribution of COVID-19 cases worldwide from the European Center for Disease Prevention and Control (ECDC). Each row/entry contains the number of new cases reported per day and per country.	
[Oxford COVID-19 Government Response Tracker](https://azure.microsoft.com/en-us/services/open-datasets/catalog/oxford-covid-19-government-response-tracker/) | The Oxford Covid-19 Government Response Tracker (OxCGRT) dataset contains systematic information on which governments have taken which measures, and when.



**_**Microsoft doesn’t own the data and use of datasets is subject to terms and conditions set by the dataset owners. Please see the details for each dataset for license and use rights_**

## Ingestion templates/pipelines - Azure Data Pipelines 

Lately, there has been a lot of interest in utilizing COVID-19 information for planning purposes, such as when to reopen stores in specific locations, or predicting supply chain impact, etc. In addition to making specific datasets available, Microsoft is providing pre-built and ready-to-go data ingestion templates that can be used to easily ingest data from other third party sources who offer open datasets, syndicated datasets or paid-license datasets. These pipeline templates can reduce in-house efforts and costs to build reliable data ingestion pipelines that keep the data up-to-date.  Additionally, if you have agreements with data providers to use data for commercial purposes, you can use these templates to pull data. 

A high-level architecture depicting the data flow is shown below: 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/arch_simple.JPG)

 

### Getting datasets using Azure Data Factory  

 

### Pre-requisites: 

* Provision Data lake on Azure Storage. Refer to documentation on [how to setup storage account and configure containers](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal). 

* Provision Azure Data Factory. Refer to documentation on [how to create Azure data factory using the portal](https://docs.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-portal). 

 

This section illustrates an example on “how to” use these prepackaged ingestion templates that allow you to quickly configure data ingestion pipeline in few easy steps. These [templates](https://github.com/Azure/azure-data-pipelines/tree/main/adf-templates) can be further easily customized to fit the needs of your data ingestion and transformation scenario specific to your business needs. 

### Instructions for setting up ‘Covid_Tracking’ pipeline in Azure Data Factory: 

1. Download this pipeline [template](https://github.com/Azure/azure-data-pipelines/blob/main/adf-templates/COVID%20Tracking%20Project/Covid_Tracking.zip) (in the form of zip files).

   Each of these pipelines will pull the raw source (pre-configured) and move it to the user-specified blob storage. It would then run data flows to 
   reformat column names. Final output will be dropped under a curated folder in various file formats (csv, json, jsonl, parquet).   

2. Create new ‘pipeline from template’, select ‘use local template’ and select ‘Covid_Tracking.zip’ file 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/PipeLine_From_Temp.png)
 

3. Select source, sink linked services 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Sink_Link_Serv.png)


4. Create source linked service (HTTP Source) 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Http_Source.png)
 

5. Create sink (Blob Storage) linked service. You can select any Blob Storage that you may have already provisioned and use it as the datalake sink.  

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/DL_Sink.png)

6. Click “Use this template” at bottom of screen 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Use_Template.png)

 

7. Once the template is loaded from the zip file, you should see the following new resources  have been created. Click on “Publish All” to ensure these resources are persisted. 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Publish_All.png)


8. Click on the ‘Parameters’ tab for the pipeline and review the default values. These values are further customizable. 

 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Parameter_Tab.png)

 

9. Optional: Click on the Data Flows on the factory resource explorer.  

   You can further review the transformations and use the visual interface to add custom logic if desired. 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Data_Flows.png)
 

 

 

10. 'Debug' run the pipeline.  You will be prompted to review the parameters for this run. 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Debug.png)

 

11. After successful execution, both (raw and curated) datasets will have been created/copied to the destination path you specified on your Azure Data Lake Store account 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Storage_Explorer.png)

 

 

12. The steps remain same for the [ECDC_Cases](https://github.com/Azure/azure-data-pipelines/blob/main/adf-templates/ECDCCases/ECDC_Cases.zip) pipeline except for the step 3 to create source linked service. Setup the source linked service for the ECDC pipeline as below: 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Edit_Link_Service.png)




## Consumption of these datasets using Azure Synapse 

 

1. Download the Synapse notebook from [Azure Open Datasets](https://azure.microsoft.com/en-us/services/open-datasets/catalog/covid-tracking/#AzureSynapse)

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/NoteBook_Open_Ds.png)
 

 

2. Import the downloaded notebook into Azure Synapse 

![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/NoteBook_Import_Synapse.png)

 

 

3. Add the creation of a Spark Database in the notebook and save dataset as Table 

 
![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Spark_Creation.png)
 
```python
%%spark
spark.sql("CREATE DATABASE IF NOT EXISTS Covid19")
df1=spark.read.parquet(wasbs_path)
df1.write.mode("overwrite").saveAsTable("Covid19.opendatatbl")
```

4. Integration of both services - you can access this Spark created table using SQL On-demand as well as metadata is shared. 

 
![](https://github.com/Azure/azure-data-pipelines/blob/main/Images/Spark_SQL_OnDemand.jpg)
  


_The team members who have contributed to this article are (in alphabetical order) Abhishek Narain, Benolin Thomas, Bill Gibson, Dinesh Srirangapatna, James Serra, Madhu Reddy Timiri, Naveed Shahzad, Pratim Das, Rahul Athale, Ravi Pedapati, Santosh Balasubramanian, Ted Malone._
