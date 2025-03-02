+++
title = "10. Clean up resources  "
date = 2025
weight = 3
chapter = false
+++

We will take the following steps to delete the resources we created in this exercise.
### Kinesis Firehose Delivery Stream ###
Go to: Kinesis Console [Click me](https://console.aws.amazon.com/firehose/home?region=us-east-1#/) 
Delete Firehose: analytics-workshop-stream
![pic](/anworkshopaws/images/10-cleanup/1.png)

### Kinesis Data Stream ###
Go to: Kinesis Console [Click me](https://console.aws.amazon.com/kinesis/home?region=us-east-1#/)  
Delete Data Stream: analytics-workshop-data-stream
![pic](/anworkshopaws/images/10-cleanup/2.png)

### Kinesis Data Analytics Studio Notebook ###
Go to: Kinesis Console [Click me](https://console.aws.amazon.com/kinesisanalytics/home?region=us-east-1#/)
Delete Notebook: AnalyticsWorkshop-KDANotebook
![pic](/anworkshopaws/images/10-cleanup/3.png)

### Lambda ###
Go to: Lambda Console [Click me](https://console.aws.amazon.com/lambda/home?region=us-east-1) 
Navigate to list of functions and select Analyticsworkshop_top5Songs.
Under Actions drop down menu, select Delete.
![pic](/anworkshopaws/images/10-cleanup/4.png)

### Glue Database ###
Go to: Glue Console [Click me](https://console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=databases) 
Delete Database: analyticsworkshopdb
![pic](/anworkshopaws/images/10-cleanup/5.png)

### Glue Crawler ###
Go to: Glue Crawlers [Click me](https://console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=crawlers) 
Delete Crawler: AnalyticsworkshopCrawler
![pic](/anworkshopaws/images/10-cleanup/6.png)

### Glue Studio Job ###
GoTo: Glue Studio Job [Click me](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/jobs) 
- Check AnalyticsOnAWS-GlueStudio
- Check AnalyticsOnAWS-GlueIS
- Check AnalyticsOnAWS-Redshift
- Click Action and choose Delete job(s)
![pic](/anworkshopaws/images/10-cleanup/7.png)

## Glue DataBrew projects ###
GoTo: Glue DataBrew projects [Click me](https://console.aws.amazon.com/databrew/home?region=us-east-1#projects)
Check AnalyticsOnAWS-GlueDataBrew
Click Action and choose Delete
Check Delete attached receipe and click Delete
![pic](/anworkshopaws/images/10-cleanup/8.png)

### Glue DataBrew datasets ###
GoTo: Glue DataBrew datasets[Click me](https://console.aws.amazon.com/databrew/home?region=us-east-1#datasets)
Check dataset name: reference-data-dataset and raw-dataset
Click Action and choose Delete
Confirm deletion by clicking Delete
![pic](/anworkshopaws/images/10-cleanup/9.png)

### Glue DataBrew Jobs ###
GoTo: Glue DataBrew Jobs[Click me](https://console.aws.amazon.com/databrew/home?region=us-east-1#jobs?tab=profile)
Check dataset name: raw-dataset profile job
Click Action and choose Delete
Confirm deletion by clicking Delete
![pic](/anworkshopaws/images/10-cleanup/10.png)

### Delete Glue connection ###
Go to: Glue Connections [Click me](https://us-east-1.console.aws.amazon.com/glue/homeregion=us-east-1#catalog:tab=connections) 
Select analytics_workshop
From Actions select Delete Connection
Click Delete
![pic](/anworkshopaws/images/10-cleanup/11.png)

### Delete IAM Role ###
Go to: IAM Console [Click me](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles) 
Search for following roles and delete:
Analyticsworkshop-GlueISRole
Analyticsworkshop_RedshiftRole
AnalyticsworkshopKinesisAnalyticsRole
Analyticsworkshop_top5Songs-role-
![pic](/anworkshopaws/images/10-cleanup/12.png)

### Delete IAM Policy ###
Go to: IAM Console [Click me](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/policies) 
Search for following policies and delete:
AWSGlueInteractiveSessionPassRolePolicy
![pic](/anworkshopaws/images/10-cleanup/13.png)

### Delete Redshift cluster ###
Go to: Redshift Console [Click me](https://console.aws.amazon.com/redshiftv2/home?region=us-east-1) 
Select redshift-cluster-1
From Actions menu, select Delete.
Uncheck Create final snapshot
Click Delete
![pic](/anworkshopaws/images/10-cleanup/14.png)
![pic](/anworkshopaws/images/10-cleanup/15.png)

### Delete S3 Gateway Endpoint ###
Go to: S3 Gateway Endpoints [Click me](https://console.aws.amazon.com/vpc/home?region=us-east-1#Endpoints:sort=vpcEndpointId) 
Select RedshiftS3EP
From Actions select Delete Endpoint
![pic](/anworkshopaws/images/10-cleanup/16.png)

### Revert Security Group rules ###
Go to: EC2 Security Groups Click me 
For the default security group:
Click on Inbound Rules
Click Edit Rules
Delete the row with S3 prefix list ID
Click Save rules
Click on Outbound rules
Click Edit Rules
Delete the self-referencing All TCP rule.
Click Save rules
![pic](/anworkshopaws/images/10-cleanup/17.png)

### Delete S3 bucket ###
Go to: S3 Console [Click me](https://s3.console.aws.amazon.com/s3/home?region=us-east-1) 
Delete Bucket: yourname-analytics-workshop-bucket
You may need to first Empty the bucket as prompted
Once emptied, proceed to Delete the bucket
![pic](/anworkshopaws/images/10-cleanup/18.png)

### Delete the Cognito CloudFormation Stack ###
Go to: CloudFormation [Click me](https://us-east-1.quicksight.aws.amazon.com/en/admin#permissions) 
Click: Kinesis-Data-Generator-Cognito-User
Click: Actions > DeleteStack
On confirmation screen:
Click Delete
![pic](/anworkshopaws/images/10-cleanup/19.png)

### Close QuickSight account ###
Go to: Quicksight Console [Click me](https://us-east-1.quicksight.aws.amazon.com/en/admin#permissions) 
Click: Unsubscribe
![pic](/anworkshopaws/images/10-cleanup/20.png)

### Cognito Userpool ###
Go to: Cognito Console [Click me](https://us-west-2.console.aws.amazon.com/cognito/users/)  
Click Kinesis Data-Generator Users
Click Delete Pool
![pic](/anworkshopaws/images/10-cleanup/21.png)