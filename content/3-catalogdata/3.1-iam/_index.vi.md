---
title : "3.1. Tạo role Iam"
date :  "2025-02-22" 
weight : 2
chapter : false
---
Trong bước này, chúng ta sẽ truy cập vào IAM Console và tạo một vai trò dịch vụ AWS Glue mới. Vai trò này cho phép AWS Glue truy cập dữ liệu lưu trữ trong S3 và tạo các thực thể cần thiết trong Glue Data Catalog.
1. Đi tới [Iam](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles)
    - Chọn **Create role**
    - Chọn dịch vụ sẽ dùng tỏng Role này: **Glue**
    ![pic](/anworkshopaws/images/3-catalogdata/1.png)
    - chọn **Next**
    - Tìm **AmazonS3FullAccess**
        - Chọn vào ô checkbox
        ![pic](/anworkshopaws/images/3-catalogdata/2.png)
    - Tìm **AWSGlueServiceRole**
        - Chọn vào ô checkbox
        ![pic](/anworkshopaws/images/3-catalogdata/3.png)
    - Chọn **Next**
    - Tên Role: **AnalyticsworkshopGlueRole**
    ![pic](/anworkshopaws/images/3-catalogdata/4.png)
    - Hãy đảm bảo rằng chỉ có 2 policies attached vào role này (**AmazonS3FullAccess, AWSGlueServiceRole**)
    - Tags, e.g.: workshop: AnalyticsOnAWS
    ![pic](/anworkshopaws/images/3-catalogdata/5.png)
    - Chọn **Create role**
