---
title : "3.1. Create Iam role"
date : "2025-02-22"
weight : 2
chapter : false
---
In this step, we will access the IAM Console to create a new AWS Glue service role. This will grant AWS Glue the permissions to access data stored in S3 and create the required entities in the Glue Data Catalog.
1. Go to [Iam](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles)
    - Click **Create role**
    - Choose the service that will use this role: **Glue**
    ![pic](/anworkshopaws/images/3-catalogdata/1.png)
    - Click **Next**
    - Search for **AmazonS3FullAccess**
        - Select the entry's checkbox
        ![pic](/anworkshopaws/images/3-catalogdata/2.png)
    - Search for **AWSGlueServiceRole**
        - Select the entry's checkbox
        ![pic](images/3-catalogdata/3.png)
    - Click Next
    - Role name: **AnalyticsworkshopGlueRole**
    ![pic](/anworkshopaws/images/3-catalogdata/4.png)
    - Make sure that only two policies attached to this role (**AmazonS3FullAccess, AWSGlueServiceRole**)
    - Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
    ![pic](/anworkshopaws/images/3-catalogdata/5.png)
    - Click **Create role**