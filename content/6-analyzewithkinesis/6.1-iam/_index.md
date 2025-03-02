---
title : "6.1 Create Iam Role"
date : "2025-02-22"
weight : 2
chapter : false
---
1. Go to [AWS IAM Role](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles)
- Click **Create role**
- Choose **Kinesis** service under Use case from drop down menu that says Use cases for other AWS services:
- Chose **Kinesis Analytics**
- Click **Next** for Permissions
![pic](/anworkshopaws/images/6-analyzewithkinesis/1.png)
- Search for **AWSGlueServiceRole** and select the entry's checkbox
- Search for **AmazonKinesisFullAccess** and select the entry's checkbox
- Click **Next**
- Enter Role name: *AnalyticsworkshopKinesisAnalyticsRole*
![pic](/anworkshopaws/images/6-analyzewithkinesis/2.png)
- Make sure that only two policies are attached to this role (**AWSGlueServiceRole, AmazonKinesisFullAccess**)
- Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
- Click **Create Role**
![pic](/anworkshopaws/images/6-analyzewithkinesis/3.png)