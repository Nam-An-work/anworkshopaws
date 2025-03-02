---
title : "4.3. Chuyển đổi dữ liệu với AWS Glue DataBrew"
date :  "2025-02-22" 
weight : 2 
chapter : false

---
![pic](/anworkshopaws/images/a-06.png) 
### AWS Glue DataBrew là gì? ###
AWS Glue DataBrew là một công cụ trực quan mới giúp các nhà phân tích dữ liệu và nhà khoa học dữ liệu dễ dàng làm sạch và chuẩn hóa dữ liệu để chuẩn bị cho phân tích và máy học. Bạn có thể chọn từ hơn 250 phép biến đổi có sẵn để tự động hóa các tác vụ chuẩn bị dữ liệu mà không cần viết bất kỳ dòng mã nào.

DataBrew giúp bạn tự động lọc các giá trị bất thường, chuyển đổi dữ liệu sang định dạng tiêu chuẩn, sửa các giá trị không hợp lệ và thực hiện nhiều tác vụ khác. Sau khi dữ liệu sẵn sàng, bạn có thể sử dụng ngay cho các dự án phân tích và máy học. Bạn chỉ trả tiền cho những gì bạn sử dụng, không có cam kết trả trước.
   
### Thực hành ###
1. Truy cập [Glue DataBrew Console](https://console.aws.amazon.com/databrew/home?region=us-east-1#landing)
   - Nhấn **Create project**
   - Nhập **Project name: AnalyticsOnAWS-GlueDataBrew**
   ![pic](/anworkshopaws/images/4-datatransformation/28.png)
   - Chọn **New dataset**
   - Nhập **raw-dataset** làm Dataset name
   ![pic](/anworkshopaws/images/4-datatransformation/29.png)
   - Chọn **All AWS Glue tables** để kết nối với Glue Catalog
   ![pic](/anworkshopaws/images/4-datatransformation/30.png)
   - Chọn **analyticsworkshopdb** và bảng **raw**
   ![pic](/anworkshopaws/images/4-datatransformation/31.png)
   - Trong **Permissions**, chọn **Create new IAM role** và đặt tên **AnalyticsOnAWS-GlueDataBrew**
   - Nhấn **Create project**
   ![pic](/anworkshopaws/images/4-datatransformation/32.png)

2. Khám phá dữ liệu trong Glue DataBrew
   - Khi phiên DataBrew được tạo, bạn sẽ thấy giao diện như hình dưới:
   ![pic](/anworkshopaws/images/4-datatransformation/33.png)
   - Nhấn **SCHEMA** để xem thông tin cột, kiểu dữ liệu và phân bố dữ liệu
   ![pic](/anworkshopaws/images/4-datatransformation/34.png)
   - Quay lại **GRID** và thay đổi kiểu dữ liệu của **track_id** thành **string**
   ![pic](/anworkshopaws/images/4-datatransformation/35.png)

3. Chạy profiling để phân tích dữ liệu
   - Nhấn **PROFILE** và chọn **Run data profile**
   ![pic](/anworkshopaws/images/4-datatransformation/36.png)
   - Giữ nguyên **Job name** và **Job run sample**
   ![pic](/anworkshopaws/images/4-datatransformation/37.png)
   - Đặt vị trí lưu trên S3: **s3://naman-analytics-workshop-bucket/**
   ![pic](/anworkshopaws/images/4-datatransformation/38.png)
   - Chọn IAM role đã tạo và nhấn **Create and run job**
   ![pic](/anworkshopaws/images/4-datatransformation/39.png)
   - Bạn sẽ thấy trạng thái job như sau:
   ![pic](/anworkshopaws/images/4-datatransformation/40.png)

4. Kết hợp dữ liệu từ nhiều bảng
   - Trở lại **GRID**, nhấn **Join**
   ![pic](/anworkshopaws/images/4-datatransformation/41.png)
   - Chọn **Connect new dataset**
   ![pic](/anworkshopaws/images/4-datatransformation/42.png)
   - Chọn **All AWS Glue tables** → **analyticsworkshopdb** → **reference_data**
   - Nhập **reference-data-dataset** làm tên dataset
   ![pic](/anworkshopaws/images/4-datatransformation/44.png)
   - Nhấn **Next**, chọn:
     - **track_id** từ **raw-dataset**
     - **track_id** từ **reference-data-dataset**
     - Bỏ chọn **track_id** từ bảng B
   - Nhấn **Finish**
   ![pic](/anworkshopaws/images/4-datatransformation/46.png)
   - Kết quả sẽ như sau:
   ![pic](/anworkshopaws/images/4-datatransformation/47.png)

5. Xem thông tin chi tiết dữ liệu
   - Nhấn **PROFILE** để xem thông tin như dữ liệu bị thiếu, giá trị trùng lặp, phân bố giá trị,...
   ![pic](/anworkshopaws/images/4-datatransformation/48.png)
   - Nhấn **LINEAGE** để xem trực quan luồng dữ liệu từ nguồn đến đầu ra
   ![pic](/anworkshopaws/images/4-datatransformation/49.png)

6. Tạo và chạy job xử lý dữ liệu
   - Quay lại **GRID**, nhấn **Create job**
   ![pic](/anworkshopaws/images/4-datatransformation/51.png)
   - Cấu hình:
     - **Job name**: AnalyticsOnAWS-GlueDataBrew-Job
     - **File type**: GlueParquet
     - **S3 location**: s3://naman-analytics-workshop-bucket/data/processed-data/
   ![pic](/anworkshopaws/images/4-datatransformation/52.png)
   - Chọn IAM role đã tạo, nhấn **Create and run job**
   ![pic](/anworkshopaws/images/4-datatransformation/53.png)
   - Bạn sẽ thấy 1 job đang chạy:
   ![pic](/anworkshopaws/images/4-datatransformation/54.png)
   - Trong **Jobs**, nhấn vào **Job name** để xem chi tiết lịch sử chạy
   ![pic](/anworkshopaws/images/4-datatransformation/55.png)
   - Khi hoàn tất (mất 4-5 phút), bạn sẽ thấy trạng thái **Succeeded**
   - Nhấn **1 output** dưới cột **Output**
   ![pic](/anworkshopaws/images/4-datatransformation/58.png)
   - Nhấn vào đường dẫn S3 để xem file đầu ra
   ![pic](/anworkshopaws/images/4-datatransformation/59.png)
   - Bạn sẽ thấy file kết quả từ Glue DataBrew!
   ![pic](/anworkshopaws/images/4-datatransformation/60.png)

Chúc mừng! Bạn đã hoàn thành bài thực hành với AWS Glue DataBrew, giúp làm sạch và chuẩn bị dữ liệu nhanh chóng, trực quan mà không cần viết mã.

