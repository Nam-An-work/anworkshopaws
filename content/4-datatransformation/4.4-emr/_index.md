---
title : "4.4. Transform Data with EMR"
date : "2025-02-22"
weight : 2
chapter : false
---
![pic](/anworkshopaws/images/a-01.png) 
### 1. Copy the script to S3 ###
In this step, we will navigate to S3 Console and create couple of folders to be used for the EMR step.
- Go to: [S3 Console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)
- Add the PySpark script:
    - Open **naman-analytics-workshop-bucket**
    - Click **Create folder**
    - Create a new folder called **scripts**
    - Click **Create folder**
    ![pic](images/4-datatransformation/61.png)
- Open **scripts**
    - Download [this file](https://static.us-east-1.prod.workshops.aws/public/252b2158-4ee1-410c-b074-58190ec31cd6/static/scripts/emr_pyspark.py) locally
    - In the S3 console, click **Upload**
    ![pic](/anworkshopaws/images/4-datatransformation/62.png)
- Create a folder for EMR logs:
    - Open **naman-analytics-workshop-bucket**
    - Click **Create folder**
    - Create a new folder called **logs**
    - Click **Create folder**
    ![pic](/anworkshopaws/images/4-datatransformation/63.png)  

### 2. Create EMR cluster and add step ###
In this step, we will create a EMR cluster and submit a Spark step.
- Go to to the [EMR console](https://us-east-1.console.aws.amazon.com/emr/home?region=us-east-1#/clusters)
- Click on **Create cluster**
- Name and applications:
    - Name: **analytics-workshop-transformer**
    - Amazon EMR release: default (e.g.: emr-6.10.0)
    - Application bundle: **Spark**
    - AWS Glue Data Catalog settings: **Use for Spark table metadata** unchecked
    - Leave the other settings to default
    ![pic](/anworkshopaws/images/4-datatransformation/64.png)
- Cluster configuration:
    - Choose Instance groups
    - Leave Primary, Core and Task to default value (m5.xlarge)
    - Leave Cluster scaling and provisioning option to default (Core: size 1, Task -1: size 1)
    ![pic](/anworkshopaws/images/4-datatransformation/65.png)
- Networking leave to default
- Step:
    -Add **Step**
    - Type: **Spark application**
    - Name: **Spark job**
    - Deploy mode: **Cluster mode**
    - Application location: s3://naman-analytics-workshop-bucket/scripts/emr_pyspark.py
    - Arguments: enter the name of your s3 bucket naman-analytics-workshop-bucket
    - Action if step fails: **Terminate cluster**
    - Click **Save step**
    ![pic](/anworkshopaws/images/4-datatransformation/66.png)
- Cluster termination: 
    - Automatically terminate cluster after idle time (Recommended)
    - Idle time: 0 days 01:00:00
    - Check Terminate cluster after last step completes
    - Uncheck Use termination protection
    ![pic](/anworkshopaws/images/4-datatransformation/67.png)
- Cluster logs:
    - Check Publish cluster-specific logs to Amazon S3
    - Amazon S3 location: s3://naman-analytics-workshop-bucket/logs/
    ![pic](/anworkshopaws/images/4-datatransformation/68.png)
- Tags: Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
    ![pic](/anworkshopaws/images/4-datatransformation/69.png)
- Identity and Access Management (IAM) roles
    - Amazon EMR service role: **Create a service role**
    - EC2 instance profile for Amazon EMR: **Create an instance profile**
    - S3 bucket access: **All S3 buckets in this account with read and write access**
    ![pic](/anworkshopaws/images/4-datatransformation/70.png)
    ![pic](/anworkshopaws/images/4-datatransformation/71.png)   
- Click **Create cluster**

### 3. Check the status of the Transform Job run on EMR ### 
- The EMR cluster will take 6-8 minute to get provisioned, and another minute or so to complete the Spark step execution.
- The Cluster will be terminated after the Spark job has been executed.
- To check the status of the job, click on the Cluster name: **analytics-workshop-transformer**
    - Go to the **Steps** tab
    - Here you should see two items: **Spark application** and **Setup hadoop debugging**
    - The status of the **Spark application** should change from Pending to Running to Completed
    ![pic](/anworkshopaws/images/4-datatransformation/72.png)
- Once the Spark job run is complete the EMR cluster will be terminated
- Under EMR > Cluster, you will see the Status of the cluster as **Terminated** with **All steps completed** message.
    ![pic](/anworkshopaws/images/4-datatransformation/73.png)

### 4. Validate - Transformed / Processed data has arrived in S3 ###
Let's go ahead and confirm that the EMR transform job has created the data set in the [S3 console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)
    - Click - **naman-analytics-workshop-bucket > data**
    - Open the new folder **emr-processed-data**
    - Ensure that **.parque**t files are created in this folder.
    ![pic](/anworkshopaws/images/4-datatransformation/74.png)

### 5. Rerun the Glue Crawler ###
Go to: [Glue console](https://console.aws.amazon.com/glue/home?region=us-east-1)
- On the left panel, click on **Crawlers**
- Select the crawler created in the previous module: **AnalyticsworkshopCrawler**
- Click on **Run**
- You should see the Status change to Starting
- Wait for few minutes for the crawler run to complete
- The crawler to display the **Tables added** as 1

You can go to the databases section on the left and confirm that **emr_processed_data** table has been added. You will now be able to query the results of the EMR job using Amazon Athena in the next module.

