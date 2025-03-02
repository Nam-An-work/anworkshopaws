---
title : "8. Cung cấp với Lambda"
date :  "2025-02-22" 
weight : 3 
chapter : false

---
![pic](/anworkshopaws/images/a-11.png) 
Trong bước này, chúng ta sẽ thực hiện tạo kết nối đến các máy chủ EC2 của chúng ta, nằm trong cả public và private subnet.
![pic](/anworkshopaws/images/a-11.png) 

### Tạo Hàm Lambda ###

- Truy cập vào: [Lambda Console](https://console.aws.amazon.com/lambda/home?region=us-east-1)
- Nhấn **Create function** (Tạo hàm)
- Chọn **Author from scratch** (Tạo từ đầu)
Dưới phần Thông tin cơ bản
- Đặt tên hàm là **Analyticsworkshop_top5Songs**
- Chọn Runtime là **Python 3.9**
- Mở rộng phần **Choose or create an execution role** dưới phần Permissions, đảm bảo chọn **Create a new role with basic Lambda permissions**.
![pic](/anworkshopaws/images/8-servewithlambda/1.png)

- Cuộn xuống phần **Function Code** và thay thế mã hiện tại trong **lambda_function.py** bằng đoạn mã Python dưới đây:
{{% notice note %}}
            import boto3
            import time
            import os

            # Các biến môi trường
            DATABASE = os.environ['DATABASE']
            TABLE = os.environ['TABLE']
            # Hằng số Top X
            TOPX = 5
            # Hằng số S3
            S3_OUTPUT = f's3://{os.environ["BUCKET_NAME"]}/query_results/'
            # Số lần thử lại
            RETRY_COUNT = 10

            def lambda_handler(event, context):
                client = boto3.client('athena')
                # biến truy vấn với hai biến môi trường và một hằng số
                query = f"""
                    SELECT track_name as \"Track Name\", 
                            artist_name as \"Artist Name\",
                            count(1) as \"Hits\" 
                    FROM {DATABASE}.{TABLE} 
                    GROUP BY 1,2 
                    ORDER BY 3 DESC
                    LIMIT {TOPX};
                """
                response = client.start_query_execution(
                    QueryString=query,
                    QueryExecutionContext={ 'Database': DATABASE },
                    ResultConfiguration={'OutputLocation': S3_OUTPUT}
                )
                query_execution_id = response['QueryExecutionId']
                # Kiểm tra trạng thái thực thi
                for i in range(0, RETRY_COUNT):
                    # Lấy Trạng thái thực thi truy vấn
                    query_status = client.get_query_execution(
                        QueryExecutionId=query_execution_id
                    )
                    exec_status = query_status['QueryExecution']['Status']['State']
                    if exec_status == 'SUCCEEDED':
                        print(f'Status: {exec_status}')
                        break
                    elif exec_status == 'FAILED':
                        raise Exception(f'STATUS: {exec_status}')
                    else:
                        print(f'STATUS: {exec_status}')
                        time.sleep(i)
                else:
                    client.stop_query_execution(QueryExecutionId=query_execution_id)
                    raise Exception('TIME OVER')
                # Lấy kết quả truy vấn
                result = client.get_query_results(QueryExecutionId=query_execution_id)
                print(result['ResultSet']['Rows'])
                # Hàm có thể trả kết quả cho ứng dụng hoặc dịch vụ của bạn
                # return result['ResultSet']['Rows']
{{% /notice %}}

![pic](/anworkshopaws/images/8-servewithlambda/2.png)

### Biến Môi Trường ###

Cuộn xuống phần **Environment variables** và thêm ba biến môi trường dưới đây.

- Key: DATABASE, Value: analyticsworkshopdb
- Key: TABLE, Value: processed_data
- Key: BUCKET_NAME, Value: yourname-analytics-workshop-bucket
![pic](/anworkshopaws/images/8-servewithlambda/3.png)

- Để mặc định **Memory (MB)** là 128 MB
- Thay đổi **Timeout** thành 10 giây.
![pic](/anworkshopaws/images/8-servewithlambda/4.png)

### Quyền Thực Thi ###

- Chọn tab **Permissions** ở trên cùng:
    - Nhấn vào liên kết **Role Name** dưới **Execution Role** để mở IAM Console trong một tab mới.
- Nhấn **Add permissions** và nhấn **Attach policies**
- Thêm hai chính sách sau (tìm kiếm trong hộp lọc, đánh dấu và nhấn **Attach policy**):
    - AmazonS3FullAccess
    - AmazonAthenaFullAccess
- Sau khi các chính sách này được đính kèm vào vai trò, đóng tab này.
![pic](/anworkshopaws/images/8-servewithlambda/5.png)

### Cấu Hình Sự Kiện Kiểm Tra ###

Hàm của chúng ta đã sẵn sàng để thử nghiệm. Triển khai hàm trước bằng cách nhấn **Deploy** dưới phần mã hàm.

Tiếp theo, hãy cấu hình sự kiện thử nghiệm giả để xem kết quả thực thi của hàm lambda mới tạo.

Nhấn **Test** ở góc trên bên phải của bảng điều khiển lambda.
- Một cửa sổ mới sẽ bật lên để chúng ta cấu hình sự kiện thử nghiệm.
    - **Create new test event** (Tạo sự kiện thử mới) được chọn mặc định.
    - Tên sự kiện: Test
    - Template: Hello World
    - Để mọi thứ mặc định
    - Nhấn **Save**
    - Nhấn **Test** lần nữa
Bạn sẽ thấy kết quả đầu ra ở định dạng JSON trong phần **Execution Result**.
![pic](/anworkshopaws/images/8-servewithlambda/6.png)

### Xác Minh Qua Athena ###

- Truy cập vào: [Athena Console](https://console.aws.amazon.com/athena/home?region=us-east-1#query)
- Ở panel bên trái, chọn **analyticsworkshopdb** từ danh sách thả xuống
- Chạy truy vấn sau
{{% notice note %}}
        SELECT track_name as "Track Name",
            artist_name as "Artist Name",
            count(1) as "Hits" 
        FROM analyticsworkshopdb.processed_data 
        GROUP BY 1,2 
        ORDER BY 3 DESC 
        LIMIT 5;
{{% /notice %}}

![pic](/anworkshopaws/images/8-servewithlambda/7.png)
