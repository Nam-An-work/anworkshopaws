---
title : "4.2. Chuyển đổi dữ liệu với AWS Glue Studio (graphical interface)"
date :  "2025-02-22" 
weight : 2 
chapter : false
---
![pic](/anworkshopaws/images/a-05.png) 
### AWS Glue Studio là gì? ###
AWS Glue Studio là giao diện đồ họa mới giúp bạn dễ dàng tạo, chạy và giám sát các tác vụ extract, transform, và load (ETL) trong AWS Glue. Bạn có thể trực quan soạn thảo quy trình chuyển đổi dữ liệu và chạy chúng liền mạch trên động cơ ETL serverless dựa trên Apache Spark của AWS Glue.

Trong bài thực hành này, chúng ta sẽ thực hiện quy trình ETL tương tự như "Chuyển đổi Dữ liệu với AWS Glue (phiên tương tác)".

Nhưng lần này, chúng ta sẽ tận dụng giao diện đồ họa trực quan trong AWS Glue Studio!

### Thực hành ###
- Truy cập vào [Glue Studio Console](https://console.aws.amazon.com/gluestudio/home?region=us-east-1)
- Nhấp vào **Jobs** và chọn Visual với blank canvas
- Nhấp vào **Create**
![pic](/anworkshopaws/images/4-datatransformation/7.png)
- Nhấp vào **Source** và chọn **S3**
![pic](/anworkshopaws/images/4-datatransformation/8.png)
- Nhấp vào tab **Data source properties - S3** trong cửa sổ cấu hình bên phải màn hình.
- Dưới **S3 source type** chọn **Data Catalog table**
  - Database - analyticsworkshopdb
  - Table - raw
  ![pic](/anworkshopaws/images/4-datatransformation/9.png)
- Bây giờ, hãy lặp lại các bước tương tự để thêm reference_data từ **S3**. Nhấp vào **Source** và chọn **S3**
- Nhấp vào tab **Data source properties - S3** trong cửa sổ cấu hình bên phải màn hình.
- Dưới **S3 source type** chọn **Data Catalog table**
  - Database - analyticsworkshopdb
  - Table - reference_data
  ![pic](/anworkshopaws/images/4-datatransformation/10.png)
- Nhấp vào node S3 đã được thêm trước đó trên canvas, sau đó nhấp vào add node và chọn **Change Schema.**
![pic](/anworkshopaws/images/4-datatransformation/11.png)
- Chọn tab Transform bên phải và thay đổi **Data type** của **track_id** thành **int**.
![pic](/anworkshopaws/images/4-datatransformation/12.png)
- Nhấp vào node S3 bên trái trên canvas, sau đó nhấp vào add node và chọn **Join**
![pic](/anworkshopaws/images/4-datatransformation/13.png)
Bạn sẽ thấy sơ đồ trực quan như trong ảnh chụp màn hình bên dưới, và thông báo bên phải **"Insufficient source nodes"** vì bạn cần thêm một node (Data source) để thực hiện phép join.
![pic](/anworkshopaws/images/4-datatransformation/14.png)
- Tiếp theo, nhấp vào node Transform - Join và trong cửa sổ cấu hình bên phải, chọn dropdown và chọn Change Schema dưới mục Transforms như trong ảnh dưới đây:
![pic](/anworkshopaws/images/4-datatransformation/15.png)
- Nhấp vào **Add condition**. Chọn cột **track_id** làm cột join như hiển thị trong ảnh dưới đây.
![pic](/anworkshopaws/images/4-datatransformation/16.png)
- Với node **Join** được chọn trên canvas, nhấp vào add node và chọn **Change Schema**
![pic](/anworkshopaws/images/4-datatransformation/17.png)
- Bạn sẽ thấy sơ đồ trực quan như trong ảnh chụp màn hình bên dưới.
![pic](/anworkshopaws/images/4-datatransformation/18.png)
- Chúng ta sẽ loại bỏ các cột không sử dụng và ánh xạ kiểu dữ liệu mới cho các cột sau:
  - Loại bỏ các cột:
    - parition_0
    - parition_1
    - parition_2
    - parition_3
  - Ánh xạ kiểu dữ liệu mới:
    - track_id: string
  ![pic](/anworkshopaws/images/4-datatransformation/19.png)
- Nhấp vào node **Transform - ApplyMapping** trên canvas.
- Nhấp vào **Target** và chọn S3 như hiển thị trong ảnh dưới đây.
![pic](/anworkshopaws/images/4-datatransformation/20.png)
- Trong **Data target properties - S3**, nhập các thông số sau:
  - Format: Parquet
  - Compression Type: Snappy
  - S3 Target Location: s3://naman-analytics-workshop-bucket/data/processed-data2/
  - Data Catalog update options:
    - Chọn "Create a table in the Data Catalog" và trong các lần chạy sau, cập nhật schema và thêm partition mới.
  - Database: analyticsworkshopdb
  - Table name: processed-data2
  ![pic](/anworkshopaws/images/4-datatransformation/21.png)
- Nhấp vào **Job details** và cấu hình với các tùy chọn sau:
  - Name: **AnalyticsOnAWS-GlueStudio**
  - IAM Role: **AnalyticsWorkshopGlueRole**
  - Requested number of workers: **2**
  - Job bookmark: **Disable**
  - Number of retries: **1**
  - Job timeout (minutes): **10**
  - Giữ các giá trị mặc định còn lại.
  - Nhấp vào **Save**
  ![pic](/anworkshopaws/images/4-datatransformation/22.png)
  ![pic](/anworkshopaws/images/4-datatransformation/23.png)
- Nhấp vào **Save** và bạn sẽ thấy thông báo **"Successfully created job"**. Bắt đầu tác vụ ETL này bằng cách nhấp vào **Run** ở góc trên bên phải màn hình.
![pic](/anworkshopaws/images/4-datatransformation/24.png)
- Chờ vài giây và bạn sẽ thấy trạng thái chạy của ETL job là **"Succeeded"** như trong ảnh chụp màn hình bên dưới.
![pic](/anworkshopaws/images/4-datatransformation/25.png)
- Bạn có thể xem mã Pyspark mà Glue Studio đã tạo ra và tái sử dụng mã này cho các mục đích khác nếu cần.
![pic](/anworkshopaws/images/4-datatransformation/26.png)
- Truy cập vào Glue DataCatalog: [Nhấn vào đây](https://console.aws.amazon.com/glue/home?region=us-east-1#) và bạn sẽ thấy bảng processed-data2 được tạo dưới database analyticsworkshopdb.
![pic](/anworkshopaws/images/4-datatransformation/27.png)
Well Done!! Bạn đã hoàn thành bài thực hành ETL bổ sung với AWS Glue Studio. Với AWS Glue Studio, bạn có thể trực quan soạn thảo quy trình chuyển đổi dữ liệu và chạy liền mạch trên động cơ ETL serverless dựa trên Apache Spark của AWS Glue.

