---
title : "6.2. Create Kinesis Data Stream"
date : "2025-02-22"
weight : 2
chapter : false
---
Kinesis Data Generator is an application that makes it simple to send test data to your Amazon Kinesis stream or Amazon Kinesis Firehose delivery stream. We will create Kinesis Data Stream to ingest the data from Kinesis Data Generator. Our Kinesis Application notebook will read streaming data from this Kinesis Data Stream.

- Go to [AWS Kinesis service](https://console.aws.amazon.com/kinesis/home?region=us-east-1)
- Click **Create Data Stream**
- Enter a data stream name: **analytics-workshop-data-stream**
- In Data Stream capacity:
- Choose Capacity Mode: **Provisioned**
- Set Provisioned Shards: 2
- Go down to bottom and click Create Data Stream
![pic](/anworkshopaws/images/6-analyzewithkinesis/4.png)