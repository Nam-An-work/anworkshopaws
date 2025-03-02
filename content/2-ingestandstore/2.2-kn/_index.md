---
title : "2.2. Create Data Firehose"
date : "2025-02-22"
weight : 2
chapter : false
---
In this step we will navigate to Data Console & create a Data Firehose delivery stream to ingest data & store in S3:
1. Go to [Kinesis Firehose Console](https://console.aws.amazon.com/firehose/home?region=us-east-1).
- Click - **Create delivery stream**.
    - Step 1: Choose source and destination
        - Source: **Direct PUT**
        - Destination: **Amazon S3**
    ![pic](/anworkshopaws/images/2-ingestandstore/9.png)
    - Step 2: Delivery stream name
        - Delivery stream name: **analytics-workshop-stream**
    ![pic](/anworkshopaws/images/2-ingestandstore/10.png)
    - Step 3: Transform and convert records
        - Transform source records with AWS Lambda: **Disabled** (Leave 'Turn on data transformation' as unchecked)
        - RecConvert record format: **Disabled** (Leave 'Enable record format conversion' as unchecked)
    ![pic](/anworkshopaws/images/2-ingestandstore/11.png)
    - Step 4: Destination settings
        - S3 bucket: **naman-analytics-workshop-bucket**
        - New line delimiter: **Not Enabled**
        - Dynamic partitioning: **Not Enabled**
    ![pic](/anworkshopaws/images/2-ingestandstore/12.png)
        - S3 bucket prefix: **data/raw/**
        - S3 bucket error output prefix: Leave Blank
    ![pic](/anworkshopaws/images/2-ingestandstore/13.png)
        - Expand **Buffer hints, compression and encryption**
            - Buffer size: **1 MiB**
            - Buffer interval: **60 seconds**
            ![pic](/anworkshopaws/images/2-ingestandstore/14.png)
            - Compression for data records: **Not Enabled**
            - Encryption for data records: **Use the encryption setting of the S3 bucket**
            ![pic](/anworkshopaws/images/2-ingestandstore/15.png)
    - Step 5: Advanced settings
            - Server-side encryption: **unchecked**
            - Amazon Cloudwatch error logging: **Enabled**
            - Permissions: **Create or update IAM role KinesisFirehoseServiceRole-xxxx**
            ![pic](/anworkshopaws/images/2-ingestandstore/16.png)
            - Optionally add Tags, e.g.: workshop - AnalyticsOnAWS
            ![pic](/anworkshopaws/images/2-ingestandstore/17.png)
    - Step 6: Review
{{% notice note %}}
Note: the slash **/** after **raw** is important. If you miss it Firehose will copy the data into an undesired location
{{% /notice %}}
