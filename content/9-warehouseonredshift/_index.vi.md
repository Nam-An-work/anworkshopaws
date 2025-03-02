---
title : "9. Kho dữ liệu trên Redshift"
date :  "2025-02-22" 
weight : 3 
chapter : false
---
![pic](/anworkshopaws/images/a-12.png) 
Trong mô-đun này, chúng ta sẽ thiết lập cụm Amazon Redshift và sử dụng AWS Glue để tải dữ liệu vào Amazon Redshift. Chúng ta sẽ tìm hiểu về một số cân nhắc về thiết kế và các phương pháp hay nhất về việc tạo và tải dữ liệu vào các bảng trong Redshift và chạy các truy vấn trên đó.

### Tạo IAM Role cho Redshift ###

Truy cập vào: [Redshift Console](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles)
- Nhấn vào **Create role** (Tạo vai trò)
- Chọn **Redshift** dưới Use case và Use cases for other AWS services:
- Chọn **Redshift - customizable**
- Nhấn Next
![pic](/anworkshopaws/images/9-warehouseonredshift/1.png)

Trong hộp tìm kiếm, tìm và chọn hai chính sách sau
- **AmazonS3FullAccess**
- **AWSGlueConsoleFullAccess**
- Nhấn Next
- Đặt tên Role là **Analyticsworkshop_RedshiftRole**
![pic](/anworkshopaws/images/9-warehouseonredshift/2.png)

- Tùy chọn thêm Tags, ví dụ: workshop: AnalyticsOnAWS
- Nhấn **Create role** (Tạo vai trò)
![pic](/anworkshopaws/images/9-warehouseonredshift/3.png)

### Tạo Cụm Redshift ###

Truy cập vào [Redshift cluster](https://console.aws.amazon.com/redshiftv2/home?region=us-east-1)
- Nhấn vào **Provision** và quản lý các cụm từ bảng điều khiển bên trái
- Nhấn vào **Create Cluster** (Tạo Cụm)
- Để **Cluster identifier** là redshift-cluster-1
- Chọn **dc2.large** làm Node Type
- Chọn **Number of Nodes** là 2.
- Kiểm tra tóm tắt cấu hình.
![pic](/anworkshopaws/images/9-warehouseonredshift/4.png)

- Thay đổi **Master user name** thành **admin**
- Nhập **Master user password** (Mật khẩu người dùng chính)
![pic](/anworkshopaws/images/9-warehouseonredshift/5.png)

- Dưới phần **Cluster permissions** (Quyền cụm)
- Nhấn **Associate IAM role** (Liên kết vai trò IAM)
- Chọn **Analyticsworkshop_RedshiftRole** đã tạo trước đó
- Nhấn **Associate IAM roles** (Liên kết vai trò IAM)
- **Analyticsworkshop_RedshiftRole** sẽ xuất hiện dưới phần **Associated IAM roles** (Vai trò IAM liên kết)
![pic](/anworkshopaws/images/9-warehouseonredshift/6.png)

- Để mặc định phần **Additional configurations** (Cấu hình bổ sung). Nó sử dụng VPC mặc định và nhóm bảo mật mặc định.
- Nhấn **Create cluster** (Tạo cụm)

### Tạo S3 Gateway Endpoint ###

Truy cập vào: [AWS VPC Console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Endpoints:)
- Nhấn **Create endpoint** (Tạo điểm cuối)
- Tên tag - tùy chọn: RedshiftS3EP
- Chọn **AWS Services** dưới **Service category** (Chuyên mục dịch vụ) (mặc định)
![pic](/anworkshopaws/images/9-warehouseonredshift/7.png)

- Dưới phần **Service name** (Tên dịch vụ), tìm kiếm **"s3"** và nhấn enter/return.
- **com.amazonaws.us-east-1.s3** sẽ xuất hiện như kết quả tìm kiếm. Chọn tùy chọn này với loại là **Gateway**
- Dưới **VPC**, chọn VPC mặc định. Đây là VPC đã được sử dụng để cấu hình cụm Redshift.
![pic](/anworkshopaws/images/9-warehouseonredshift/8.png)

- Sau khi kiểm tra lại ID VPC, tiếp tục cấu hình phần **Route tables**.
- Chọn bảng định tuyến đã liệt kê (đây sẽ là bảng định tuyến chính). Bạn có thể kiểm tra điều này bằng cách xem **Yes** dưới cột **Main**.
- Để mặc định **Policy** (Chính sách) là **Full access** (Toàn quyền).
![pic](/anworkshopaws/images/9-warehouseonredshift/9.png)

- Tùy chọn thêm Tags, ví dụ: * workshop: AnalyticsOnAWS
![pic](/anworkshopaws/images/9-warehouseonredshift/10.png)

- Nhấn **Create endpoint** (Tạo điểm cuối). Quá trình này sẽ mất vài giây để hoàn thành. Khi điểm cuối này đã sẵn sàng, bạn sẽ thấy trạng thái là - **Available** (Có sẵn) đối với điểm cuối S3 mới tạo.

### Xác Minh và Thêm Quy Tắc vào Nhóm Bảo Mật Mặc Định ###

Truy cập vào: [VPC Security Groups](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#SecurityGroups:)
- Chọn nhóm bảo mật Redshift. Nó sẽ là nhóm bảo mật mặc định nếu không thay đổi trong bước tạo cụm Redshift.
![pic](/anworkshopaws/images/9-warehouseonredshift/11.png)

- Nhấn **Edit inbound rules** (Chỉnh sửa quy tắc đến)
![pic](/anworkshopaws/images/9-warehouseonredshift/12.png)

- Nhấn **Edit outbound rules** (Chỉnh sửa quy tắc đi)
![pic](/anworkshopaws/images/9-warehouseonredshift/13.png)

### Tạo Kết Nối Redshift dưới Kết Nối Glue ###

Truy cập vào: [Glue Connections Console](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/connectors)
- Dưới phần **Connections** (Kết nối), nhấn **Create connection** (Tạo kết nối)
- Đặt tên **Connection name** là **analytics_workshop**.
- Chọn **Connection** loại là **Amazon Redshift**.
![pic](/anworkshopaws/images/9-warehouseonredshift/14.png)

Cấu hình truy cập Kết Nối
- Chọn **Database instances** là **redshift-cluster-1**
- Tên cơ sở dữ liệu và Tên người dùng sẽ được tự động điền là **dev** và **admin** tương ứng.
- Mật khẩu: Nhập mật khẩu đã sử dụng khi thiết lập cụm Redshift.
- Nhấn **Create connection** (Tạo kết nối)
![pic](/anworkshopaws/images/9-warehouseonredshift/15.png)

### Tạo Schema và Bảng Redshift ###

Truy cập vào: [Redshift Query Editor](https://us-east-1.console.aws.amazon.com/redshiftv2/home?region=us-east-1#query-editor:)
- Nhấn **Connect** (Kết nối) vào cơ sở dữ liệu
- **Connection**: Tạo kết nối mới
- **Authentication**: Temporary credentials (Chứng thực tạm thời)
- **cluster**: redshift-cluster-1
- **Database name**: dev
- **Database user**: admin
- Nhấn **Connect** (Kết nối)
![pic](/anworkshopaws/images/9-warehouseonredshift/16.png)
- Thực thi các truy vấn dưới đây để tạo schema và bảng cho dữ liệu thô và dữ liệu tham chiếu.

{{% notice note %}}

            CREATE schema redshift_lab;
 {{% /notice %}}

{{% notice note %}}
            --    Tạo bảng f_raw_1.
            CREATE TABLE IF not EXISTS redshift_lab.f_raw_1 (
            uuid            varchar(256),
            device_ts         timestamp,
            device_id        int,
            device_temp        int,
            track_id        int,
            activity_type        varchar(128),
            load_time        int
            );
 {{% /notice %}}

 {{% notice note %}}
            --    Tạo bảng d_ref_data_1.
            CREATE TABLE IF NOT EXISTS redshift_lab.d_ref_data_1 (
            track_id        int,
            track_name    varchar(128),
            artist_name    varchar(128)
            );
 {{% /notice %}}
![pic](/anworkshopaws/images/9-warehouseonredshift/17.png)

### Sử dụng Jupyter Notebook trong AWS Glue để phát triển ETL tương tác ###

Tải và lưu tệp này vào máy tính của bạn [file](https://static.us-east-1.prod.workshops.aws/public/252b2158-4ee1-410c-b074-58190ec31cd6/static/notebooks/analytics-workshop-redshift-glueis-notebook.ipynb)
- Truy cập vào: [Glue Studio jobs](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/jobs)
- Chọn tùy chọn **Jupyter Notebook**
- Chọn **Upload and edit an existing notebook**
- Nhấn **Choose file**
- Duyệt và tải lên **analytics-workshop-redshift-glueis-notebook.ipynb** mà bạn đã tải về trước đó
- Nhấn **Create**
![pic](/anworkshopaws/images/9-warehouseonredshift/18.png)

Dưới phần Cài đặt Notebook và Cấu hình ban đầu
- Tên job: AnalyticsOnAWS-Redshift
- IAM role: Analyticsworkshop-GlueISRole
- Để mặc định Kernel là Spark
- Nhấn **Start notebook**

Thực thi các truy vấn dưới đây để kiểm tra số lượng bản ghi trong bảng dữ liệu thô và dữ liệu tham chiếu.
{{% notice note %}}
        select count(1) from redshift_lab.f_raw_1;

        select count(1) from redshift_lab.d_ref_data_1;
 {{% /notice %}}

  {{% notice note %}}
        select 
        track_name, 
        artist_name, 
        count(1) frequency
        from 
        redshift_lab.f_raw_1 fr
        inner join 
        redshift_lab.d_ref_data_1 drf
        on 
        fr.track_id = drf.track_id
        where 
        activity_type = 'Running'
        group by 
        track_name, artist_name
        order by 
        frequency desc
        limit 10;
 {{% /notice %}}


