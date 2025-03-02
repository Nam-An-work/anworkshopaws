---
title : "3.3. Truy vấn dữ liệu đã nhập bằng Amazon Athena."
date :  "2025-02-22" 
weight : 2 
chapter : false
---
Hãy truy vấn dữ liệu vừa được nhập vào bằng Amazon Athena.
1. Vào [AWS Athena](https://us-east-1.console.aws.amazon.com/athena/home?region=us-east-1#query)
   - Nếu cần, click **Edit settings** trong thông báo màu xanh ở gần đầu giao diện Athena
   ![pic](/anworkshopaws/images/3-catalogdata/13.png)
   - Click tab **Editor**
   - Trong bảng bên trái (**Database**) chọn **analyticsworkshopdb** > chọn bảng **raw**
   - Click vào **3 chấm** (3 dấu chấm dọc) > Chọn **Preview Table**
   - Xem lại kết quả
   - Trong query editor, dán câu truy vấn sau:
            {{% notice note %}}
            SELECT activity_type, count(activity_type)
            FROM raw
            GROUP BY  activity_type
            ORDER BY  activity_type
            {{% /notice %}}
   - Click **Run**
   ![pic](/anworkshopaws/images/3-catalogdata/14.png)
