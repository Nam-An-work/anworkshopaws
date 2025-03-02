---
title : "2.1 Tạo S3 bucket"
date :  "2025-02-22" 
weight : 2
chapter : false
---
Đi đến S3 Console và tạo một bucket mới trong khu vực us-east-1:

1. Truy cập vào [S3 Console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1).
    - Nhấn vào - **Create Bucket**.
        - Tên bucket - **yourname-analytics-workshop-bucket**
        ![pic](/anworkshopaws/images/2-ingestandstore/2.png)
        - Khu vực - **US EAST (N. Virginia)**.
        - Tùy chọn thêm Tags, ví dụ:
            - workshop: AnalyticsOnAWS
            ![pic](/anworkshopaws/images/2-ingestandstore/2.1.png)
        - Nhấn vào - **Create Bucket**.

2. Thêm dữ liệu tham khảo:
    - Mở **yourname-analytics-workshop-bucket**
    ![pic](/anworkshopaws/images/2-ingestandstore/3.png)
        - Nhấn vào - **Create folder**
        - Tạo thư mục mới có tên - **data**
        - Nhấn vào - **Create folder**
        ![pic](/anworkshopaws/images/2-ingestandstore/4.png)
    - Mở - **data**
        - Nhấn vào - **Create folder** (Từ trong thư mục data)
        - Thư mục mới: **reference_data**
        ![pic](/anworkshopaws/images/2-ingestandstore/5.png)
        - Nhấn vào - **Create folder**
        ![pic](/anworkshopaws/images/2-ingestandstore/6.png)
    - Mở - **reference_data**
        - Tải tệp này xuống máy tính: [tracks_list.json](https://static.us-east-1.prod.workshops.aws/public/252b2158-4ee1-410c-b074-58190ec31cd6/static/data/tracks_list.json)
        - Trong S3 Console, nhấn vào - **Upload**
            - Nhấn vào **Add files** và tải tệp **tracks_list.json** lên đây
            ![pic](/anworkshopaws/images/2-ingestandstore/7.png)
            - Nhấn vào **Upload** (góc dưới bên trái)
            ![pic](/anworkshopaws/images/2-ingestandstore/8.png)
