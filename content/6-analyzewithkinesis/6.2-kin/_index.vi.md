---
title : "6.2. Tạo Kinesis Data Stream"
date :  "2025-02-22" 
weight : 2 
chapter : false
---
Kinesis Data Generator là một ứng dụng giúp bạn dễ dàng gửi dữ liệu thử nghiệm đến luồng dữ liệu Amazon Kinesis hoặc luồng phân phối Amazon Kinesis Firehose. Chúng ta sẽ tạo một Kinesis Data Stream để tiếp nhận dữ liệu từ Kinesis Data Generator. Notebook Kinesis Application của chúng ta sẽ đọc dữ liệu luồng từ Kinesis Data Stream này.

- Truy cập vào [Dịch vụ AWS Kinesis](https://console.aws.amazon.com/kinesis/home?region=us-east-1)
- Nhấn **Create Data Stream** (Tạo Luồng Dữ Liệu)
- Nhập tên luồng dữ liệu: **analytics-workshop-data-stream**
- Trong phần Capacity của Data Stream:
- Chọn Capacity Mode: **Provisioned**
- Cài đặt Provisioned Shards: 2
- Cuộn xuống dưới cùng và nhấn **Create Data Stream** (Tạo Luồng Dữ Liệu)
![pic](/anworkshopaws/images/6-analyzewithkinesis/4.png)
