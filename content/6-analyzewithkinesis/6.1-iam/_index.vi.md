---
title : "6.1. Tạo Role Iam"
date :  "2025-02-22" 
weight : 2
chapter : false
---
1. Truy cập vào [AWS IAM Role](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles)
- Nhấn **Create role** (Tạo vai trò)
- Chọn dịch vụ **Kinesis** trong phần Use case từ menu thả xuống có dòng chữ Use cases for other AWS services:
- Chọn **Kinesis Analytics**
- Nhấn **Next** để tiếp tục với phần Permissions
![pic](/anworkshopaws/images/6-analyzewithkinesis/1.png)
- Tìm kiếm **AWSGlueServiceRole** và chọn ô checkbox tương ứng
- Tìm kiếm **AmazonKinesisFullAccess** và chọn ô checkbox tương ứng
- Nhấn **Next**
- Nhập tên Vai trò: *AnalyticsworkshopKinesisAnalyticsRole*
![pic](/anworkshopaws/images/6-analyzewithkinesis/2.png)
- Đảm bảo rằng chỉ có hai chính sách được gắn với vai trò này (**AWSGlueServiceRole, AmazonKinesisFullAccess**)
- Tùy chọn thêm Tags, ví dụ: workshop: AnalyticsOnAWS
- Nhấn **Create Role** (Tạo Vai trò)
![pic](/anworkshopaws/images/6-analyzewithkinesis/3.png)


