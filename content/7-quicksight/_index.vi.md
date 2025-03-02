---
title : "7. Trực quan hóa trong Amazon QuickSight"
date :  "2025-02-22" 
weight : 3 
chapter : false
---
![pic](/anworkshopaws/images/a-10.png) 
Trong mô-đun này, chúng ta sẽ sử dụng Amazon Quicksight để xây dựng một số hình ảnh trực quan về dữ liệu được thu thập và lưu trữ trong S3.
### Cài Đặt QuickSight ### 
Trong bước này, chúng ta sẽ trực quan hóa dữ liệu đã xử lý bằng QuickSight.

- Truy cập vào: [Quicksight Console](https://us-east-1.quicksight.aws.amazon.com/en/start)
- Nhấn **Sign up for QuickSight** (Đăng ký QuickSight)
- Đảm bảo chọn **Enterprise** và nhấn **Continue** (Tiếp tục)
- Tên tài khoản QuickSight: yournameanalyticsworkshop
- Địa chỉ email thông báo: you@youremail.com
- Chọn **Amazon Athena** - điều này cho phép QuickSight truy cập vào cơ sở dữ liệu Amazon Athena
- Chọn **Amazon S3**
- Chọn **naman-analytics-workshop-bucket**
- Nhấn **Finish**
Chờ cho tài khoản QuickSight của bạn được tạo
![pic](/anworkshopaws/images/7-quicksight/1.png)

### Thêm Dữ Liệu Mới ### 
- Truy cập vào [quicksight](https://us-east-1.quicksight.aws.amazon.com/sn/start/data-sets)
- Ở góc trên bên phải, nhấn **New Analyze** (Phân tích Mới)
- Ở góc trên bên trái, nhấn **New Dataset** (Dữ liệu Mới)
![pic](/anworkshopaws/images/7-quicksight/2.png)
- Nhấn **Athena**
**Nguồn dữ liệu Athena mới**
- Tên nguồn dữ liệu: analyticsworkshop
- Nhấn **Validate connection** (Xác nhận kết nối)
- Nhấn **Create data source** (Tạo nguồn dữ liệu)
![pic](/anworkshopaws/images/7-quicksight/3.png)

**Chọn bảng của bạn**
- Cơ sở dữ liệu: chứa các bộ bảng - chọn **analyticsworkshopdb**
- Bảng: chứa dữ liệu bạn có thể trực quan hóa - chọn **processed_data**
- Nhấn **Select** (Chọn)
**Hoàn thành tạo bộ dữ liệu**
- Chọn **Directly query your data** (Truy vấn trực tiếp dữ liệu của bạn)
- Nhấn **Visualize** (Trực quan hóa)
![pic](/anworkshopaws/images/7-quicksight/4.png)

### Sử Dụng Amazon QuickSight để Trực Quan Hóa Dữ Liệu Đã Xử Lý ### 
Ở bảng phía dưới bên trái (Các loại hình trực quan):
- Di chuột qua các biểu tượng để xem tên các hình trực quan có sẵn
- Nhấn vào **Heat Map** (Bản đồ nhiệt)
Ở bảng phía trên bên trái (Danh sách các trường):
- Nhấn **device_id**
- Nhấn **track_name**
Ngay trên hình trực quan, bạn sẽ thấy **Field wells**: Dòng: device_id | Cột: track_name
![pic](/anworkshopaws/images/7-quicksight/5.png)




