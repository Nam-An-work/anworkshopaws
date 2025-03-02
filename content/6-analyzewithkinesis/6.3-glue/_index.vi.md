---
title : "6.3. Tạo bảng Glue Catalog"
date :  "2025-02-22" 
weight : 2 
chapter : false
---
Notebook Kinesis Application của chúng ta sẽ lấy thông tin về nguồn dữ liệu từ AWS Glue. Khi bạn tạo notebook Studio, bạn chỉ định cơ sở dữ liệu AWS Glue chứa thông tin kết nối của bạn. Khi truy cập vào các nguồn dữ liệu, bạn chỉ định các bảng AWS Glue có trong cơ sở dữ liệu.

- Truy cập vào [AWS Glue](https://console.aws.amazon.com/glue/home?region=us-east-1)
- Từ thanh bên trái, vào phần Databases và nhấn vào cơ sở dữ liệu chúng ta đã tạo trước đó **analyticsworkshopdb**
- Nhấn **Tables in analyticsworkshopdb**
- Nhấn menu thả xuống **Add tables** và chọn **Add table manually** \

**Trong Table Properties**
- Nhập tên bảng: **raw_stream**
![pic](/anworkshopaws/images/6-analyzewithkinesis/5.png)

**Trong Data store**
- Chọn loại nguồn dữ liệu: Kinesis
- Bỏ qua phần **Select a kinesis data stream** (Luồng trong tài khoản của tôi sẽ được chọn mặc định)
- Khu vực: US East (N. Virginia) us-east-1
- Tên luồng Kinesis: analytics-workshop-data-stream
- Bỏ qua phần kích thước mẫu
![pic](/anworkshopaws/images/6-analyzewithkinesis/6.png)

**Trong Data format**
- Classification: JSON
- Sau đó nhấn **Next**
![pic](/anworkshopaws/images/6-analyzewithkinesis/7.png)

**Trong Schema**
- Nhấn **Edit Schema as JSON** và chèn đoạn văn bản JSON sau:
      {{% notice note %}}
      [{
         "Name": "uuid",
         "Type": "string",
         "Comment": ""
      },
      {
         "Name": "device_ts",
         "Type": "timestamp",
         "Comment": ""
      },
      {
         "Name": "device_id",
         "Type": "int",
         "Comment": ""
      },
      {
         "Name": "device_temp",
         "Type": "int",
         "Comment": ""
      },
      {
         "Name": "track_id",
         "Type": "int",
         "Comment": ""
      },
      {
         "Name": "activity_type",
         "Type": "string",
         "Comment": ""
      }]
      {{% /notice %}}
![pic](/anworkshopaws/images/6-analyzewithkinesis/8.png)
- Sau đó nhấn **Next**

**Bỏ qua Partition Indices** \
**Review and create**

- Xác minh rằng bảng Glue mới **raw_stream** đã được tạo chính xác. Làm mới danh sách bảng nếu bảng chưa xuất hiện.
- Nhấn vào bảng mới tạo **raw_stream**
- Nhấn **Actions** và nhấn **Edit table**
   - Trong phần Table properties, thêm thuộc tính mới:
   - Key: kinesisanalytics.proctime
   - Value: proctime
![pic](/anworkshopaws/images/6-analyzewithkinesis/9.png)