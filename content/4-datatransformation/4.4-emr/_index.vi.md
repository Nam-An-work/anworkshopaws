---
title : "4.4. Chuyển đổi dữ liệu với EMR"
date :  "2025-02-22" 
weight : 2 
chapter : false
---
![pic](/anworkshopaws/images/a-07.png) 
### 1. Tải script lên S3 ###
Trong bước này, chúng ta sẽ tạo các thư mục trên S3 để sử dụng trong bước chạy EMR.
- Truy cập: [S3 Console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)
- Thêm script PySpark:
    - Mở **naman-analytics-workshop-bucket**
    - Nhấn **Create folder** → đặt tên **scripts** → Nhấn **Create folder**
    ![pic](/anworkshopaws/images/4-datatransformation/61.png)
- Mở **scripts**
    - Tải xuống [file này](https://static.us-east-1.prod.workshops.aws/public/252b2158-4ee1-410c-b074-58190ec31cd6/static/scripts/emr_pyspark.py)
    - Trong S3 console, nhấn **Upload**
    ![pic](/anworkshopaws/images/4-datatransformation/62.png)
- Tạo thư mục lưu log EMR:
    - Mở **naman-analytics-workshop-bucket**
    - Nhấn **Create folder** → đặt tên **logs** → Nhấn **Create folder**
    ![pic](/anworkshopaws/images/4-datatransformation/63.png)

### 2. Tạo EMR cluster và thêm bước xử lý ###
- Truy cập [EMR console](https://us-east-1.console.aws.amazon.com/emr/home?region=us-east-1#/clusters)
- Nhấn **Create cluster**
- Cấu hình cluster:
    - **Cluster name**: analytics-workshop-transformer
    - **Amazon EMR release**: mặc định (ví dụ: emr-6.10.0)
    - **Application bundle**: Spark
    - **AWS Glue Data Catalog settings**: bỏ chọn **Use for Spark table metadata**
    - Giữ nguyên các cài đặt khác
    ![pic](/anworkshopaws/images/4-datatransformation/64.png)
- Cấu hình tài nguyên:
    - **Instance groups**: Giữ nguyên mặc định (m5.xlarge)
    - **Cluster scaling**: Giữ nguyên (Core: 1, Task -1: 1)
    ![pic](/anworkshopaws/images/4-datatransformation/65.png)
- Giữ nguyên phần **Networking**
- Thêm bước xử lý:
    - Nhấn **Add Step**
    - **Type**: Spark application
    - **Name**: Spark job
    - **Deploy mode**: Cluster mode
    - **Application location**: s3://naman-analytics-workshop-bucket/scripts/emr_pyspark.py
    - **Arguments**: nhập tên bucket S3 **naman-analytics-workshop-bucket**
    - **Action if step fails**: Terminate cluster
    - Nhấn **Save step**
    ![pic](/anworkshopaws/images/4-datatransformation/66.png)
- Cấu hình tự động tắt cluster:
    - **Idle time**: 0 days 01:00:00
    - Chọn **Terminate cluster after last step completes**
    - Bỏ chọn **Use termination protection**
    ![pic](/anworkshopaws/images/4-datatransformation/67.png)
- Cấu hình log:
    - Chọn **Publish cluster-specific logs to Amazon S3**
    - Nhập S3 location: s3://naman-analytics-workshop-bucket/logs/
    ![pic](/anworkshopaws/images/4-datatransformation/68.png)
- **IAM roles**:
    - Chọn **Create a service role** cho Amazon EMR
    - Chọn **Create an instance profile** cho EC2 instance profile
    - S3 bucket access: **All S3 buckets in this account with read and write access**
    ![pic](/anworkshopaws/images/4-datatransformation/70.png)
- Nhấn **Create cluster**

### 3. Kiểm tra trạng thái job EMR ###
- EMR cluster sẽ mất khoảng 6-8 phút để khởi tạo và 1 phút để chạy Spark job.
- Sau khi hoàn thành, cluster sẽ tự động tắt.
- Để kiểm tra trạng thái job:
    - Truy cập **Cluster name: analytics-workshop-transformer**
    - Chuyển đến tab **Steps**
    - Bạn sẽ thấy **Spark application** và **Setup hadoop debugging**
    - Trạng thái **Spark application** sẽ thay đổi từ **Pending → Running → Completed**
    ![pic](/anworkshopaws/images/4-datatransformation/72.png)
- Sau khi hoàn tất, trạng thái EMR Cluster sẽ là **Terminated** với thông báo **All steps completed**.
    ![pic](/anworkshopaws/images/4-datatransformation/73.png)

### 4. Xác nhận dữ liệu đã xử lý trên S3 ###
- Truy cập [S3 console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)
- Kiểm tra dữ liệu đầu ra:
    - Mở **naman-analytics-workshop-bucket > data**
    - Mở thư mục **emr-processed-data**
    - Kiểm tra file **.parquet** đã được tạo trong thư mục này.
    ![pic](/anworkshopaws/images/4-datatransformation/74.png)

### 5. Chạy lại Glue Crawler ###
- Truy cập: [Glue console](https://console.aws.amazon.com/glue/home?region=us-east-1)
- Trong menu bên trái, nhấn **Crawlers**
- Chọn **AnalyticsworkshopCrawler**
- Nhấn **Run**
- Trạng thái sẽ thay đổi từ **Starting** → **Running**
- Sau vài phút, crawler sẽ hoàn thành với **Tables added** = 1
- Kiểm tra trong mục **Databases** để xác nhận bảng **emr_processed_data** đã được thêm vào.
- Bây giờ, bạn có thể sử dụng Amazon Athena để truy vấn kết quả từ job EMR.