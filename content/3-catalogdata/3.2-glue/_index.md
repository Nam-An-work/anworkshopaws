---
title : "3.2. Create AWS Glue Crawlers"
date : "2025-02-22"
weight : 2
chapter : false
---
In this step, we will go to the AWS Glue Console and create Glue crawlers to discover the schema of the newly ingested data in S3.
1. Go to [AWS Glue](https://console.aws.amazon.com/glue/home?region=us-east-1)
   - On the left panel at **Data Catalog**, click on **Crawlers**
   - Click on **Create crawler**, Crawler info:
      - Crawler name: **AnalyticsworkshopCrawler**
      - Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
      - Click **Next**
      ![pic](/anworkshopaws/images/3-catalogdata/6.png)
   - Click **Add a data source**
   - Choose a **Data source**:
      - Data source: **S3**
      - Leave **Network connection - optional**
      - Select **In this account** under **Location of S3 data**
      - Include S3 path: s3://naman-analytics-workshop-bucket/data/
      - Leave **Subsequent crawler runs** to default selection of **Crawl all sub-folders**
      - Click **Add an S3 data source**
      ![pic](/anworkshopaws/images/3-catalogdata/7.png)
      - Select recently added S3 data source under **Data Sources**
      - Click **Next**
   - IAM Role
      - Under **Existing IAM role**, select AnalyticsworkshopGlueRole
      - Leave everything else as-is.
      - Click Next
      ![pic](/anworkshopaws/images/3-catalogdata/7.png)
   - Output configuration
      - Click **Add database** to bring up a new window for creating a database
      - Database details:
         - Name: *analyticsworkshopdb*
         - Click **Create database**
      - Closes the current window and returns to the previous window.
      - Refresh by clicking the refresh icon to the right of the **Target database**
      - Choose *analyticsworkshopdb* under **Target database**
      - Under **Crawler schedule**
         - Frequency: On demand
         - Click **Next**
      ![pic](/anworkshopaws/images/3-catalogdata/9.png)  
      - Review all settings under Review and create
      - Click **Create crawler**
   - You should see this message: **The following crawler is now created: "AnalyticsworkshopCrawler"**
   ![pic](/anworkshopaws/images/3-catalogdata/10.png)  
      - Click **Run crawler** to run the crawler for the first time
      - Wait for few minutes
   ![pic](/anworkshopaws/images/3-catalogdata/11.png)
2. Verify newly created tables in catalog
Navigate to Glue Catalog and explore the crawled data:
   - Go to [AWS Glue](https://console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=databases)
   - Click *analyticsworkshopdb*
   - Click **Tables in analyticsworkshopdb**
      - Click **raw**
      - Look around and explore the schema for your dataset
      ![pic](/anworkshopaws/images/3-catalogdata/12.png)
         - Look for the *averageRecordSize, recordCount, compressionType*
