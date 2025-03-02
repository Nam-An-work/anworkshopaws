---
title : "3.2. Tạo AWS Glue Crawlers"
date :  "2025-02-22" 
weight : 2 
chapter : false
---
Trong bước này, chúng ta sẽ truy cập vào AWS Glue Console và tạo các Glue Crawlers để phát hiện schema của dữ liệu mới được nạp vào S3.
1. Đi tới [AWS Glue](https://console.aws.amazon.com/glue/home?region=us-east-1)
   - Ở bên trái tại **Data Catalog**, Chọn **Crawlers**
   - Chọn **Create crawler**, Thông tin Crawler:
      - Crawler name: **AnalyticsworkshopCrawler**
      - Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
      - Chọn **Next**
      ![pic](/anworkshopaws/images/3-catalogdata/6.png)
   - Chọn **Add a data source**
   - Chọn **Data source**:
      - Data source: **S3**
      - Để trống **Network connection - optional**
      - Chọn **In this account** ở dưới **Location of S3 data**
      - Include S3 path: s3://naman-analytics-workshop-bucket/data/
      - Ở **Subsequent crawler runs** để mặc định **Crawl all sub-folders**
      - Chọn **Add an S3 data source**
      ![pic](/anworkshopaws/images/3-catalogdata/7.png)
      - Chọn added S3 data source ở dưới **Data Sources**
      - chọn **Next**
   - IAM Role
      - Dưới **Existing IAM role**, chọn AnalyticsworkshopGlueRole
      - Để trống mọi thứ
      - chọn Next
      ![pic](/anworkshopaws/images/3-catalogdata/7.png)
   - Output configuration
      - Chọn **Add database** Để xuất hiện cửa sổ mới tạo database
      - Database details:
         - Name: *analyticsworkshopdb*
         - Click **Create database**
      - Đóng cửa sổ hiện tại và quay về cửa sổ trước đó
      - Làm mới icon bên phải **Target database**
      - Chọn *analyticsworkshopdb* dưới **Target database**
      - Ở dưới **Crawler schedule**
         - Frequency: On demand
         - Click **Next**
      ![pic](/anworkshopaws/images/3-catalogdata/9.png)  
      - Review lại các cài đặt
      - Chọn **Create crawler**
   - Bạn nên thấy thông báo: **The following crawler is now created: "AnalyticsworkshopCrawler"**
   ![pic](/anworkshopaws/images/3-catalogdata/10.png)  
      - Chọn **Run crawler** để crawl data lần đầu
      - chờ vài phút
   ![pic](/anworkshopaws/images/3-catalogdata/11.png)
2. Xác thực bảng đã được tạo mới trong datalog
Vào Glue Catalog và khám phá data đã được crawl:
   - Vào[AWS Glue](https://console.aws.amazon.com/glue/home?region=us-east-1#catalog:tab=databases)
   - Chọn *analyticsworkshopdb*
   - Chọn **Tables in analyticsworkshopdb**
      - Chọn **raw**
      - Khám phá cấu trúc dữ liệu
      ![pic](/anworkshopaws/images/3-catalogdata/12.png)
         - Look for the *averageRecordSize, recordCount, compressionType*