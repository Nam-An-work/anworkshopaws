---
title : "5. Phân tích với Athena"
date :  "2025-02-22" 
weight : 3 
chapter : false
---
![pic](/anworkshopaws/images/a-08.png) 
Trong bước này, chúng ta sẽ thực hiện tạo kết nối đến các máy chủ EC2 của chúng ta, nằm trong cả public và private subnet.
#### 1. Đăng nhập vào Amazon Athena Console
- Truy cập: [Athena Console](https://console.aws.amazon.com/athena/home?region=us-east-1#query)
- Athena sử dụng AWS Glue Catalog để quản lý nguồn dữ liệu, vì vậy bất kỳ bảng nào được hỗ trợ bởi S3 trong Glue đều có thể được truy vấn qua Athena.

#### 2. Chạy truy vấn phân tích dữ liệu
- Trong bảng điều khiển bên trái, chọn **analyticsworkshopdb** từ menu dropdown.
- Khám phá giao diện Athena và thử chạy một số truy vấn. Hãy thử truy vấn bảng emr_processed_data.
            {{% notice note %}}
        SELECT artist_name, count(artist_name) AS count
        FROM processed_data
        GROUP BY artist_name
        ORDER BY count desc
            {{% /notice %}}
- Truy vấn này trả về danh sách các bài hát được phát lại nhiều lần trên các thiết bị.
           {{% notice note %}}
        SELECT device_id,
        track_name,
        count(track_name) AS count
        FROM processed_data
        GROUP BY device_id, track_name
        ORDER BY count desc
            {{% /notice %}}
- Xem bảng
  ![pic](/anworkshopaws/images/5-analyzewithathena/1.png)

