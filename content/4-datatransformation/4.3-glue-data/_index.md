---
title : "4.3. Transform Data with AWS Glue DataBrew"
date : "2025-02-22"
weight : 2
chapter : false

---
![pic](/anworkshopaws/images/a-06.png) 
### What is AWS Glue DataBrew ###
   AWS Glue DataBrew is a new visual data preparation tool that makes it easy for data analysts and data scientists to clean and normalize data to prepare it for analytics and machine learning. You can choose from over 250 pre-built transformations to automate data preparation tasks, all without the need to write any code. You can automate filtering anomalies, converting data to standard formats, and correcting invalid values, and other tasks. After your data is ready, you can immediately use it for analytics and machine learning projects. You only pay for what you use - no upfront commitment.

### Practice ###
1. Go To [Glue Databrew Console](https://console.aws.amazon.com/databrew/home?region=us-east-1#landing)
   - Click - **Create project**
   - Enter **Project name: AnalyticsOnAWS-GlueDataBrew** as shown in the screenshot below
   ![pic](/anworkshopaws/images/4-datatransformation/28.png)
   - Under **Select a dataset** choose **New dataset**
      - In New dataset details, enter **raw-dataset** as Dataset name as shown in the screenshot below.
      ![pic](/anworkshopaws/images/4-datatransformation/29.png)
   - Under Connect to new dataset, select **All AWS Glue tables** , you should see all databases in AWS Glue Catalog as shown in the screenshot below
   ![pic](/anworkshopaws/images/4-datatransformation/30.png)
   - Click **analyticsworkshopdb**, choose **raw table**
   ![pic](/anworkshopaws/images/4-datatransformation/31.png)
   - Under **Permissions**
      - Choose **Role name** as **Create new IAM role**
      - Enter **AnalyticsOnAWS-GlueDataBrew** in **New IAM role suffix**
      - Click **Create project**
      ![pic](/anworkshopaws/images/4-datatransformation/32.png)
   - Once Glue DataBrew session is created, you should see like in the screenshot below:
      ![pic](/anworkshopaws/images/4-datatransformation/33.png)
   - Click **SCHEMA** tab on the right hand top corner of the screen to explore table schema and its properties such as column name, data type, data quality, value distribution, and box plot distribution for numeric values.
      ![pic](/anworkshopaws/images/4-datatransformation/34.png)
   - Click GRID tab to return to grid view, We will change **track_id** data type, by click # in track_id column, and choose **string** type as shown in the following screenshot
      ![pic](/anworkshopaws/images/4-datatransformation/35.png)
   - Now, lets profile the data to unearth informative statistics like correlation between attributes. Click **PROFILE** tab on the right hand top corner of the screen, and click **Run data** profile
      ![pic](/anworkshopaws/images/4-datatransformation/36.png)
   - Leave **Job name**, and **Job run sample** as default option
      ![pic](/anworkshopaws/images/4-datatransformation/37.png)
   - Specify S3 location as your bucket name **s3://naman-analytics-workshop-bucket/** for job output. Do not forget to replace portion in the s3 path. Leave **Enable encryption for job output file** option unchecked.
      ![pic](/anworkshopaws/images/4-datatransformation/38.png)
   - Under **Permissions**, select the role as **Role name** that was created previously in this module.
   - Click Create and run job
      ![pic](/anworkshopaws/images/4-datatransformation/39.png)
   - You should get similar output as shown in the screenshot below which means that Glue Databrew has already started profiling your data
      ![pic](/anworkshopaws/images/4-datatransformation/40.png)
   - Click **GRID** tab to return to grid view
   - Click **Join**
      ![pic](/anworkshopaws/images/4-datatransformation/41.png)
   - Click **Connect new dataset**
      ![pic](/anworkshopaws/images/4-datatransformation/42.png)
   - Click **All AWS Glue tables**, and click **analyticsworkshopdb**
      ![pic](/anworkshopaws/images/4-datatransformation/43.png)
   - Click **reference_data**
   - Dataset name - **reference-data-dataset**
      ![pic](/anworkshopaws/images/4-datatransformation/44.png)
   - You should see similar screen as shown in the screenshot below, click **Next**
      ![pic](/anworkshopaws/images/4-datatransformation/45.png)
   - Select **track_id** from **raw-dataset**
   - Select **track_id** from **reference-data-set**
   - Unselect **track_id** from **Table B**
   - Click **Finish**
      ![pic](/anworkshopaws/images/4-datatransformation/46.png)
   - You should see result as showin in the screenshot below:
      ![pic](/anworkshopaws/images/4-datatransformation/47.png)
   - Click **PROFILE** to review your raw dataset profiling result such as summary, missing cells, duplicate rows, correlations, value distribution, and columns statistics, it will give you deeper level of insights about your data
      ![pic](/anworkshopaws/images/4-datatransformation/48.png)
   - Click **LINEAGE** on the top right corner
      ![pic](/anworkshopaws/images/4-datatransformation/49.png)
   - You should be able to see the data lineage, that is visually represented to provide understanding of data flow with involved transformation at all steps from source to sink.
      ![pic](/anworkshopaws/images/4-datatransformation/50.png)
   - Go back to the **GRID** view, and click **Create job**
      ![pic](/anworkshopaws/images/4-datatransformation/51.png)
   - Fill in the following values:
      - Under **Job details**
         - Job name: **AnalyticsOnAWS-GlueDataBrew-Job
      - Under **Job output settings**
         - File type: **GlueParquet**
         - S3 location: **s3://naman-analytics-workshop-bucket/data/processed-data/**
      ![pic](/anworkshopaws/images/4-datatransformation/52.png)
   - Scroll down to **Permission**, and choose role name that you have created in first step, and click **Create and run job**
      ![pic](/anworkshopaws/images/4-datatransformation/53.png)
   - You should see 1 job in progress
      ![pic](/anworkshopaws/images/4-datatransformation/54.png)
   - Click Jobs on the left menu, you should see following screenshot, then click **Job name** (Hyperlink)
      ![pic](/anworkshopaws/images/4-datatransformation/55.png)
   - Here you can explore job run history, job detail, and data lineage like in the screeshot below:
      ![pic](/anworkshopaws/images/4-datatransformation/56.png)
      ![pic](/anworkshopaws/images/4-datatransformation/57.png)
   - This job should take around 4-5 minutes to be completed, you should see **Succeeded** status, and click **1 output** under **Output** columnn.
      ![pic](/anworkshopaws/images/4-datatransformation/58.png)
   - You should be able to see output destination under **Destination** column, click on S3 location (hyperlink) provided in this column
      ![pic](/anworkshopaws/images/4-datatransformation/59.png)
   - You should see output file from Glue Databrew job!
      ![pic](/anworkshopaws/images/4-datatransformation/60.png)


