---
title : "4.2. Transform Data with AWS Glue Studio (graphical interface)"
date : "2025-02-22"
weight : 2
chapter : false
---
![pic](/anworkshopaws/images/a-05.png) 
### What is AWS Glue Studio? ###
AWS Glue Studio is a new graphical interface that makes it easy to create, run, and monitor extract, transform, and load (ETL) jobs in AWS Glue. You can visually compose data transformation workflows and seamlessly run them on AWS Glue’s Apache Spark-based serverless ETL engine.

In this lab, We will do the same ETL process like Transform Data with AWS Glue (interactive sessions)

But This time We will leverage visual graphical interface in AWS Glue Studio!
### Practice ###
   - Go to [Glue Studio Console](https://console.aws.amazon.com/gluestudio/home?region=us-east-1)
      - Click - **Jobs** and choose Visual with a blank canvas
      - Click **Create**
      ![pic](/anworkshopaws/images/4-datatransformation/7.png)
      - Click - **Source** and choose - **S3**
      ![pic](/anworkshopaws/images/4-datatransformation/8.png)
      - Click tab **Data source properties - S3** in configuration window on right side of the screen.
      - Under **S3 source type** choose **Data Catalog table**
         - Database - analyticsworkshopdb
         - Table - raw
         ![pic](/anworkshopaws/images/4-datatransformation/9.png)
      - Now lets repeat same steps to add reference_data from **S3**. Click - **Source** and choose - **S3**
      - Click tab **Data source properties - S3** in configuration window on right side of the screen.
      - Under **S3 source type** choose **Data Catalog table**
         - Database - analyticsworkshopdb
         - Table - reference_data
         ![pic](/anworkshopaws/images/4-datatransformation/10.png)
      - Click the S3 node added earlier in the canvas, then click add node and choose **Change Schema.**
      ![pic](/anworkshopaws/images/4-datatransformation/11.png)
      - Select the Transform tab on the right and change the **Data type** of **track_id** to **int**.
      ![pic](/anworkshopaws/images/4-datatransformation/12.png)
      - Click the left S3 node in the canvas, then click add node and choose **Join**
      ![pic](/anworkshopaws/images/4-datatransformation/13.png)
      You should get the visual diagram like in the screenshot below, and message on the right **"Insufficient source nodes"** because you need another node (Data source) to join
      ![pic](/anworkshopaws/images/4-datatransformation/14.png)
      -  Next, click on Transform - Join node and on Node properties on the right configuration window, select dropdown list and and select Change Schema under Tramsforms as depicted in the screenshot below:
      ![pic](/anworkshopaws/images/4-datatransformation/15.png)
      - Click **Add condition**. Choose **track_id** column as join column as shown in the screenshot below.
      ![pic](/anworkshopaws/images/4-datatransformation/16.png)
      - With **Join** node selected on canvas, click add node and choose **Change Schema**
      ![pic](/anworkshopaws/images/4-datatransformation/17.png)
      - You should get visual diagram like in the screenshot below
      ![pic](/anworkshopaws/images/4-datatransformation/18.png) 
      - We will drop unused columns and map new data type for following columns:
         - drop Columns:
            - parition_0
            - parition_1
            - parition_2
            - parition_3
         - Map New Data Type:
            - track_id string
            ![pic](/anworkshopaws/images/4-datatransformation/19.png)
      - Click **Transform - ApplyMapping** node on the canvas
      - Click **Target** and choose S3 as shown in the screenshot below
      - ![pic](/anworkshopaws/images/4-datatransformation/20.png)
      - In **Data target properties - S3**, provide inputs as following:
         - Format: Parquet
         - Compression Type: Snappy
         - S3 Target Location: s3://naman-analytics-workshop-bucket/data/processed-data2/
         - Data Catalog update options
         - Choose Create a table in the Data Catalog and on subsequent runs, update the schema and add new partitions
         - Database: analyticsworkshopdb
         - Table name: processed-data2
         ![pic](/anworkshopaws/images/4-datatransformation/21.png) 
      - Click **Job details** and configure with following options
         - Name: **AnalyticsOnAWS-GlueStudio**
         - IAM Role: **AnalyticsWorkshopGlueRole**
         - Requested number of workers: **2**
         - Job bookmark: **Disable**
         - Number of retries: **1**
         - Job timeout (minutes): **10**
         - Leave the rest as default value
         - Click **Save**
         ![pic](/anworkshopaws/images/4-datatransformation/22.png)
         ![pic](/anworkshopaws/images/4-datatransformation/23.png)
      - Click Save and you should see **"Successfully created job"**. Start this ETL job by clicking **Run** on upper right hand corner of the screen.
      ![pic](/anworkshopaws/images/4-datatransformation/24.png)
      - Wait for a few seconds and you should see your ETL job Run Status **"Succeeded"** as shown in the screenshot below.
      ![pic](/anworkshopaws/images/4-datatransformation/25.png)
      - You can see the Pyspark Code that Glue Studio has generated and reuse this code for other purposes, if needed.
      ![pic](/anworkshopaws/images/4-datatransformation/26.png)
   - Go to Glue DataCatalog: [Click me](https://console.aws.amazon.com/glue/home?region=us-east-1#)  and you should see processed-data2 table created under analyticsworkshopdb database
   ![pic](/anworkshopaws/images/4-datatransformation/27.png)
Well Done!! You have finished an extra ETL lab with AWS Glue Studio. With AWS Glue Studio, you can visually compose data transformation workflows and seamlessly run them on AWS Glue’s Apache Spark-based serverless ETL engine.