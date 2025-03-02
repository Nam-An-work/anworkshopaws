---
title : "2.3. Tạo dữ liệu giả"
date :  "2025-02-22" 
weight : 2
chapter : false
---
Trong bước này, chúng ta sẽ cấu hình **Kinesis Data Generator** để tạo dữ liệu giả và đưa nó vào **Kinesis Firehose**.

1. **Cấu hình Amazon Cognito cho Kinesis Data Generator** - Trong bước này, chúng ta sẽ khởi chạy một stack CloudFormation để cấu hình Cognito. Script CloudFormation này sẽ khởi chạy ở khu vực N.Virginia (Không cần thay đổi khu vực này)
    - Truy cập vào AWS [CloudFormation](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=Kinesis-Data-Generator-Cognito-User&templateURL=https://aws-kdg-tools-us-east-1.s3.amazonaws.com/cognito-setup.yaml)
    - Nhấn vào - **Next**
    ![pic](/anworkshopaws/images/2-ingestandstore/18.png)
    - Cung cấp Chi tiết:
        - Username: admin
            - Password: **choose an alphanumeric password**
            - Nhấn vào **Next**
    ![pic](/anworkshopaws/images/2-ingestandstore/19.png)
    - Các Tùy chọn:
        - Tùy chọn thêm Tags, ví dụ: workshop: AnalyticsOnAWS
        - Nhấn vào - **Next**
    ![pic](/anworkshopaws/images/2-ingestandstore/20.png)
    - Cuộn xuống
        - Tôi xác nhận rằng AWS CloudFormation có thể tạo tài nguyên IAM: **Check**
    ![pic](/anworkshopaws/images/2-ingestandstore/21.png)
    - Nhấn vào **Next and Submit**
    - Làm mới Console CloudFormation của bạn
    - Đợi đến khi trạng thái stack thay đổi thành **Create_Complete**
    - Chọn stack **Kinesis-Data-Generator-Cognito-User**
    ![pic](/anworkshopaws/images/2-ingestandstore/22.png)
    - Vào tab outputs: nhấn vào liên kết có tên: **KinesisDataGeneratorUrl** - Liên kết này sẽ mở công cụ **Kinesis Data Generator**
    ![pic](/anworkshopaws/images/2-ingestandstore/23.png)

2. Trên trang chủ **Amazon Kinesis Data Generator**
    - Đăng nhập với tên người dùng & mật khẩu từ bước trước
    ![pic](/anworkshopaws/images/2-ingestandstore/24.png)
    - Khu vực: **us-east-1**
    - Stream/delivery stream: **analytics-workshop-stream**
    - Số bản ghi mỗi giây: **2000**
    ![pic](/anworkshopaws/images/2-ingestandstore/25.png)
    - Mẫu bản ghi (Template 1): Trong **vùng văn bản lớn**, chèn mẫu json sau:
            {{% notice note %}}
            {
                "uuid": "{{random.uuid}}",
                "device_ts": "{{date.utc("YYYY-MM-DD HH:mm:ss.SSS")}}",
                "device_id": {{random.number(50)}},
                "device_temp": {{random.weightedArrayElement(
                {"weights":[0.30, 0.30, 0.20, 0.20],"data":[32, 34, 28, 40]}
                )}},
                "track_id": {{random.number(30)}},  
                "activity_type": {{random.weightedArrayElement(
                    {
                        "weights": [0.1, 0.2, 0.2, 0.3, 0.2],
                        "data": ["\"Running\"", "\"Working\"", "\"Walking\"", "\"Traveling\"", "\"Sitting\""]
                    }
                )}}
            }
            {{% /notice %}}
    ![pic](/anworkshopaws/images/2-ingestandstore/26.png)
    - Nhấn vào - **Send Data**
    Khi công cụ gửi khoảng ~10,000 tin nhắn, bạn có thể nhấn vào - **Stop sending data to Kinesis**

3. Kiểm tra xem dữ liệu đã đến S3 hay chưa
- Sau một vài phút, truy cập vào [S3 console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)
- Điều hướng đến: **naman-analytics-workshop-bucket > data**
- Sẽ có một thư mục gọi là **raw** > Mở thư mục đó và tiếp tục điều hướng, bạn sẽ nhận thấy Firehose đã đưa dữ liệu vào S3 bằng cách phân vùng theo định dạng **yyyy/mm/dd/hh**
![pic](/anworkshopaws/images/2-ingestandstore/27.png)

Nếu bạn đã nhận được dữ liệu giả trong các bucket S3 của mình, chúng ta có thể tiếp tục bước tiếp theo!


