---
title : "6.4. Tạo Sổ tay Ứng dụng Phân tích Dữ liệu Luồng trong Studio"
date :  "2025-02-22" 
weight : 2 
chapter : false
---
### Tạo Analytics Streaming Application Studio Notebook ### 
Bây giờ chúng ta hãy tạo Kinesis Analytics Streaming Application Studio Notebook. Notebook ứng dụng phân tích này có thể xử lý dữ liệu luồng từ Kinesis Data Stream và chúng ta có thể viết các truy vấn SQL để có được những cái nhìn thời gian thực như số lượng hoạt động hiện tại hoặc nhiệt độ thiết bị.

- Truy cập vào [Managed Apache Flink](https://us-east-1.console.aws.amazon.com/kinesisanalytics/home?region=us-east-1#/list/notebooks)
- Vào phần **Studio**
- Nhấn **Create Studio notebook** (Tạo notebook Studio)
- Chọn **Create with custom settings** (Tạo với cài đặt tùy chỉnh)
![pic](/anworkshopaws/images/6-analyzewithkinesis/10.png)

**General**
- Nhập tên Studio notebook: **AnalyticsWorkshop-KDANotebook**
- Nhập Runtime: **Apache Flink 1.15, Apache Zeppelin 0.10**
- Nhấn **Next**
![pic](/anworkshopaws/images/6-analyzewithkinesis/11.png)

**IAM role**
- Chọn **Choose from IAM roles that Kinesis Data Analytics can assume** (Chọn từ các vai trò IAM mà Kinesis Data Analytics có thể giả định)
- Chọn vai trò dịch vụ đã tạo trước đó: **AnalyticsworkshopKinesisAnalyticsRole**
- Trong cơ sở dữ liệu AWS Glue, chọn: **analyticsworkshopdb**
- Nhấn **Next**
![pic](/anworkshopaws/images/6-analyzewithkinesis/12.png)

**Trong Configurations**
- Parallelism: 4
- Parallelism per KPU: 1
- Bỏ chọn ô **Turn on logging**
- Bỏ qua các cài đặt khác và đi xuống dưới cùng, trong tag: workshop: AnalyticsOnAWS
![pic](/anworkshopaws/images/6-analyzewithkinesis/13.png)

### Chạy Kinesis Analytics Studio Notebook ### 
Bây giờ chúng ta đã tạo notebook, chúng ta có thể chạy nó và thử thực hiện một số truy vấn SQL.

- Xem danh sách notebook trong tab Studio và nhấn vào notebook vừa tạo **AnalyticsWorkshop-KDANotebook**
- Nhấn **Run**
- Chờ đến khi trạng thái chuyển sang chế độ **Running**. (Mất khoảng 5-7 phút)
![pic](/anworkshopaws/images/6-analyzewithkinesis/14.png)
- Nhấn **Open in Apache Zeppelin** ở góc trên bên phải để mở Zeppelin Notebook
- Nhấn **Create new note** và đặt tên cho note là **AnalyticsWorkshop-ZeppelinNote**
- Dán truy vấn SQL này
 {{% notice note %}}
      %flink.ssql(type=update)
      SELECT * FROM raw_stream;
 {{% /notice %}}
![pic](/anworkshopaws/images/6-analyzewithkinesis/15.png)
- Nhấn **Add Paragraph**
![pic](/anworkshopaws/images/6-analyzewithkinesis/16.png)
- Dán truy vấn SQL này
 {{% notice note %}}
      %flink.ssql(type=update)
      SELECT activity_type, count(*) as activity_cnt FROM raw_stream group by activity_type;
 {{% /notice %}}
![pic](/anworkshopaws/images/6-analyzewithkinesis/17.1.png)
- Khi tất cả các truy vấn đã được dán, nhấn nút **Play** ở góc trên bên phải của đoạn văn
![pic](/anworkshopaws/images/6-analyzewithkinesis/18.png)

LƯU Ý: Sẽ chưa có dữ liệu hiển thị vì notebook Analytics Streaming này xử lý dữ liệu luồng. Chúng ta phải gửi dữ liệu luồng từ Kinesis Data Generator (nếu chưa làm), để notebook có thể hiển thị kết quả của các truy vấn.

### Tạo Dữ Liệu Giả cho Kinesis Data Stream ### 
Để hiển thị dữ liệu từ các truy vấn trong Analytics Streaming notebook, chúng ta phải gửi dữ liệu thô từ Kinesis Data Generator.

- Truy cập vào KinesisDataGeneratorURL. Bạn có thể tìm thấy điều này trong [tab Output của Cloudformation stack](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/outputs?filteringStatus=active&filteringText=&viewNested=true&hideStacks=false&stackId=arn%3Aaws%3Acloudformation%3Aus-east-1%3A860873776111%3Astack%2FKinesis-Data-Generator-Cognito-User%2Fe8fc0040-b69e-11ec-bd11-0e017e3a47f9).
![pic](/anworkshopaws/images/6-analyzewithkinesis/19.png)
- Đăng nhập với tên người dùng và mật khẩu của bạn
![pic](/anworkshopaws/images/6-analyzewithkinesis/20.png)

Điền thông tin sau:
- Khu vực: us-east-1
- Stream/delivery stream: analytics-workshop-data-stream (KHÔNG chọn analytics-workshop-stream mà bạn có thể đã tạo trong module "Ingest and Store" của khóa học này)
- Đảm bảo **Records per second** là **Constant**.
- Giá trị cho **Records per second**: 100 (KHÔNG thay đổi số này cho khóa học.)
- Đảm bảo **Compress Records** không được chọn.
- Template Record (Template 1): Trong khu vực văn bản lớn, chèn mẫu JSON sau:
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

- Sau đó nhấn **Send Data**
![pic](/anworkshopaws/images/6-analyzewithkinesis/21.png)
- Quay lại Zeppelin Notebook của chúng ta. Chờ một vài phút, kết quả sẽ hiển thị
![pic](/anworkshopaws/images/6-analyzewithkinesis/22.png)
