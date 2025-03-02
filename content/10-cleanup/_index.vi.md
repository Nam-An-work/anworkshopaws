+++
title = "10. Dọn dẹp tài nguyên  "
date = 2025
weight = 3
chapter = false
+++

Chúng ta sẽ thực hiện các bước sau để xóa các tài nguyên đã tạo trong bài tập này.

### Kinesis Firehose Delivery Stream ###
Truy cập vào: Kinesis Console [Click me](https://console.aws.amazon.com/firehose/home?region=us-east-1#/)  
Xóa Firehose: analytics-workshop-stream
![pic](/anworkshopaws/images/10-cleanup/1.png)

### Kinesis Data Stream ###
Truy cập vào: Kinesis Console [Click me](https://console.aws.amazon.com/kinesis/home?region=us-east-1#/)  
Xóa Data Stream: analytics-workshop-data-stream
![pic](/anworkshopaws/images/10-cleanup/2.png)

### Kinesis Data Analytics Studio Notebook ###
Truy cập vào: Kinesis Console [Click me](https://console.aws.amazon.com/kinesisanalytics/home?region=us-east-1#/)  
Xóa Notebook: AnalyticsWorkshop-KDANotebook
![pic](/anworkshopaws/images/10-cleanup/3.png)

### Lambda ###
Truy cập vào: Lambda Console [Click me](https://console.aws.amazon.com/lambda/home?region=us-east-1)  
Đi tới danh sách các hàm và chọn Analyticsworkshop_top5Songs.
Dưới menu Actions, chọn Delete.
![pic](/anworkshopaws/images/10-cleanup/4.png)

### Glue Database ###
Truy cập vào: Glue Console [Click me](https://console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=databases)  
Xóa Database: analyticsworkshopdb
![pic](/anworkshopaws/images/10-cleanup/5.png)

### Glue Crawler ###
Truy cập vào: Glue Crawlers [Click me](https://console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=crawlers)  
Xóa Crawler: AnalyticsworkshopCrawler
![pic](/anworkshopaws/images/10-cleanup/6.png)

### Glue Studio Job ###
Truy cập vào: Glue Studio Job [Click me](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/jobs)  
- Chọn AnalyticsOnAWS-GlueStudio
- Chọn AnalyticsOnAWS-GlueIS
- Chọn AnalyticsOnAWS-Redshift
- Nhấn Action và chọn Delete job(s)
![pic](/anworkshopaws/images/10-cleanup/7.png)

## Glue DataBrew projects ###
Truy cập vào: Glue DataBrew projects [Click me](https://console.aws.amazon.com/databrew/home?region=us-east-1#projects)  
Chọn AnalyticsOnAWS-GlueDataBrew  
Nhấn Action và chọn Delete  
Chọn Delete attached recipe và nhấn Delete
![pic](/anworkshopaws/images/10-cleanup/8.png)

### Glue DataBrew datasets ###
Truy cập vào: Glue DataBrew datasets[Click me](https://console.aws.amazon.com/databrew/home?region=us-east-1#datasets)  
Chọn tên dataset: reference-data-dataset và raw-dataset  
Nhấn Action và chọn Delete  
Xác nhận xóa bằng cách nhấn Delete
![pic](/anworkshopaws/images/10-cleanup/9.png)

### Glue DataBrew Jobs ###
Truy cập vào: Glue DataBrew Jobs[Click me](https://console.aws.amazon.com/databrew/home?region=us-east-1#jobs?tab=profile)  
Chọn tên dataset: raw-dataset profile job  
Nhấn Action và chọn Delete  
Xác nhận xóa bằng cách nhấn Delete
![pic](/anworkshopaws/images/10-cleanup/10.png)

### Xóa Glue connection ###
Truy cập vào: Glue Connections [Click me](https://us-east-1.console.aws.amazon.com/glue/homeregion=us-east-1#catalog:tab=connections)  
Chọn analytics_workshop  
Từ Actions, chọn Delete Connection  
Nhấn Delete
![pic](/anworkshopaws/images/10-cleanup/11.png)

### Xóa IAM Role ###
Truy cập vào: IAM Console [Click me](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles)  
Tìm kiếm các role sau và xóa:
Analyticsworkshop-GlueISRole  
Analyticsworkshop_RedshiftRole  
AnalyticsworkshopKinesisAnalyticsRole  
Analyticsworkshop_top5Songs-role-
![pic](/anworkshopaws/images/10-cleanup/12.png)

### Xóa IAM Policy ###
Truy cập vào: IAM Console [Click me](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/policies)  
Tìm kiếm các policy sau và xóa:
AWSGlueInteractiveSessionPassRolePolicy
![pic](/anworkshopaws/images/10-cleanup/13.png)

### Xóa Redshift cluster ###
Truy cập vào: Redshift Console [Click me](https://console.aws.amazon.com/redshiftv2/home?region=us-east-1)  
Chọn redshift-cluster-1  
Từ menu Actions, chọn Delete.  
Bỏ chọn Create final snapshot  
Nhấn Delete
![pic](/anworkshopaws/images/10-cleanup/14.png)
![pic](/anworkshopaws/images/10-cleanup/15.png)

### Xóa S3 Gateway Endpoint ###
Truy cập vào: S3 Gateway Endpoints [Click me](https://console.aws.amazon.com/vpc/home?region=us-east-1#Endpoints:sort=vpcEndpointId)  
Chọn RedshiftS3EP  
Từ Actions, chọn Delete Endpoint
![pic](/anworkshopaws/images/10-cleanup/16.png)

### Hoàn tác các quy tắc của Security Group ###
Truy cập vào: EC2 Security Groups Click me  
Với security group mặc định:  
Nhấn vào Inbound Rules  
Nhấn Edit Rules  
Xóa dòng với S3 prefix list ID  
Nhấn Save rules  
Nhấn vào Outbound rules  
Nhấn Edit Rules  
Xóa quy tắc All TCP tự tham chiếu.  
Nhấn Save rules
![pic](/anworkshopaws/images/10-cleanup/17.png)

### Xóa S3 bucket ###
Truy cập vào: S3 Console [Click me](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)  
Xóa Bucket: yourname-analytics-workshop-bucket  
Bạn có thể cần phải dọn dẹp bucket trước khi xóa.  
Sau khi dọn dẹp, tiến hành xóa bucket
![pic](/anworkshopaws/images/10-cleanup/18.png)

### Xóa Cognito CloudFormation Stack ###
Truy cập vào: CloudFormation [Click me](https://us-east-1.quicksight.aws.amazon.com/en/admin#permissions)  
Nhấn: Kinesis-Data-Generator-Cognito-User  
Nhấn: Actions > DeleteStack  
Trên màn hình xác nhận:  
Nhấn Delete
![pic](/anworkshopaws/images/10-cleanup/19.png)

### Đóng tài khoản QuickSight ###
Truy cập vào: Quicksight Console [Click me](https://us-east-1.quicksight.aws.amazon.com/en/admin#permissions)  
Nhấn: Unsubscribe
![pic](/anworkshopaws/images/10-cleanup/20.png)

### Xóa Cognito Userpool ###
Truy cập vào: Cognito Console [Click me](https://us-west-2.console.aws.amazon.com/cognito/users/)  
Nhấn Kinesis Data-Generator Users  
Nhấn Delete Pool
![pic](/anworkshopaws/images/10-cleanup/21.png)
