---
title : "4.1. Chuyển đổi dữ liệu với AWS Glue"
date :  "2025-02-22" 
weight : 2 
chapter : false
---
![pic](/anworkshopaws/images/a-04.png) 
1. Chuẩn bị chính sách IAM và vai trò  
Trong bước này, bạn sẽ truy cập vào bảng điều khiển IAM và tạo các chính sách IAM cùng vai trò cần thiết để làm việc với AWS Glue Studio Jupyter notebooks và interactive sessions. Hãy bắt đầu bằng cách tạo một chính sách IAM cho vai trò AWS Glue notebook.  

- Truy cập vào [IAM](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/policies)  
- Nhấp vào **Policies** trong menu bên trái  
- Nhấp vào **Create policy**  
    - Chuyển sang tab **JSON**  
    - Thay thế nội dung mặc định trong trình chỉnh sửa chính sách bằng đoạn mã sau:  

    {{% notice note %}}
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": "iam:PassRole",
                    "Resource": "arn:aws:iam::180294200626:role/Analyticsworkshop-GlueISRole"
                },
                {
                    "Effect": "Allow",
                    "Action": [
                        "glue:*",
                        "s3:ListBucket",
                        "s3:GetObject",
                        "s3:PutObject",
                        "s3:DeleteObject",
                        "s3:GetBucketLocation",
                        "s3:ListAllMyBuckets",
                        "s3:GetLifecycleConfiguration",
                        "s3:GetBucketPolicy",
                        "s3:GetBucketCORS"
                    ],
                    "Resource": "*"
                }
            ]
        }
    {{% /notice %}}

{{% notice note %}}
Lưu ý rằng **Analyticsworkshop-GlueISRole** là vai trò mà chúng ta sẽ tạo cho AWS Glue Studio Jupyter notebook ở bước tiếp theo.
{{% /notice %}}

{{% notice warning %}}
Cảnh báo: Hãy thay thế **AWS account ID** trong đoạn mã trên bằng ID tài khoản AWS của bạn.
{{% /notice %}}

![pic](/anworkshopaws/images/4-datatransformation/1.png)  

- Nhấp vào **Next**  
- Đặt tên chính sách: *AWSGlueInteractiveSessionPassRolePolicy*  
- (Tùy chọn) Viết mô tả cho chính sách:  
    - **Mô tả**: Chính sách này cho phép vai trò AWS Glue notebook được sử dụng trong các phiên tương tác.  
    ![pic](/anworkshopaws/images/4-datatransformation/2.png)  
- (Tùy chọn) Thêm thẻ (Tags), ví dụ: `workshop: AnalyticsOnAWS`  
    ![pic](/anworkshopaws/images/4-datatransformation/3.png)  

Tiếp theo, tạo một vai trò IAM cho AWS Glue notebook.  
- Truy cập vào [IAM Roles](https://us-east-1.console.aws.amazon.com/iamv2/home#/roles)  
- Nhấp vào **Roles** trong menu bên trái  
- Nhấp vào **Create Role**  
    - Chọn dịch vụ sẽ sử dụng vai trò này: **Glue** dưới mục "Use Case"  
    ![pic](/anworkshopaws/images/4-datatransformation/4.png)  
    - Nhấp vào **Next**  
    - Tìm kiếm và chọn các chính sách sau:  

    {{% notice note %}}
    - AWSGlueServiceRole  
    - AwsGlueSessionUserRestrictedNotebookPolicy  
    - AWSGlueInteractiveSessionPassRolePolicy  
    - AmazonS3FullAccess  
    {{% /notice %}}

- Nhấp vào **Next**  
- Đặt tên vai trò: *Analyticsworkshop-GlueISRole*  
    ![pic](/anworkshopaws/images/4-datatransformation/5.png)  
- Đảm bảo chỉ có bốn chính sách được gán cho vai trò này (**AWSGlueServiceRole, AwsGlueSessionUserRestrictedNotebookPolicy, AWSGlueInteractiveSessionPassRolePolicy, AmazonS3FullAccess**)  
- (Tùy chọn) Thêm thẻ (Tags), ví dụ: `workshop: AnalyticsOnAWS`  
- Nhấp vào **Create role**  

{{% notice note %}}
Lưu ý: Trong workshop này, chúng ta đã cấp quyền truy cập toàn bộ S3 cho vai trò Glue. Tuy nhiên, trong các triển khai thực tế, bạn nên cấp quyền tối thiểu cần thiết để thực hiện công việc, tuân theo nguyên tắc "least-privilege permissions model".  
{{% /notice %}}
2. Sử dụng Jupyter Notebook trong AWS Glue để phát triển ETL tương tác
Trong bước này, bạn sẽ tạo một AWS Glue Job với Jupyter Notebook để phát triển tập lệnh ETL trong Glue bằng PySpark.
    - Tải xuống và lưu file sau vào máy tính của bạn: [analytics-workshop-glueis-notebook.ipynb](https://static.us-east-1.prod.workshops.aws/public/252b2158-4ee1-410c-b074-58190ec31cd6/static/notebooks/analytics-workshop-glueis-notebook.ipynb)
    - Truy cập: [Glue Studio jobs](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/jobs)
    - Chọn **Jupyter Notebook**
        - Chọn **Upload and edit an existing notebook**
        - Nhấn **Choose file**
        - Tải lên **analytics-workshop-glueis-notebook.ipynb** vừa tải xuống
        - Nhấn **Create**
    ![pic](/anworkshopaws/images/4-datatransformation/6.png)
    - Trong phần **Cấu hình Notebook**:
        - Tên Job: *AnalyticsOnAWS-GlueIS*
        - IAM Role: *Analyticsworkshop-GlueISRole*
        - Kernel để mặc định là **Spark**
        - Nhấn **Start notebook**
Sau khi notebook khởi động, hãy làm theo hướng dẫn bên trong notebook.

3. Xác minh - Dữ liệu đã được xử lý có trong S3
Sau khi tập lệnh ETL chạy thành công, quay lại giao diện điều khiển S3: [Nhấn vào đây](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)
    - Mở thư mục **naman-analytics-workshop-bucket > data**
    - Truy cập vào thư mục **processed-data**:
        - Kiểm tra xem các file **.parquet** đã được tạo chưa.
Bây giờ dữ liệu đã được xử lý, bạn có thể truy vấn bằng **Amazon Athena** hoặc tiếp tục xử lý/tổng hợp với **AWS Glue hoặc Amazon EMR**.
Bước tiếp theo về **EMR là tùy chọn**, bạn có thể bỏ qua và tiếp tục đến **Phân tích với Athena**.


