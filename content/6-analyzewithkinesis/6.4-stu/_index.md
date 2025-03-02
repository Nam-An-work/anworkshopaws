---
title : "6.4. Create Analytics Streaming Application Studio Notebook"
date : "2025-02-22"
weight : 2
chapter : false
---
### Create Analytics Streaming Application Studio Notebook ###
Now let's create our Kinesis Analytics Streaming Application Studio Notebook. This Analytics application notebook can process streaming data from Kinesis Data Stream and we can write SQL analytical queries to get real-time insights such as current activity count or device temperature.
- Go to [Managed Apache Flink](https://us-east-1.console.aws.amazon.com/kinesisanalytics/home?region=us-east-1#/list/notebooks)
- Navigate to **Studio**
- Click **Create Studio notebook**
- Choose **Create with custom settings**
![pic](/anworkshopaws/images/6-analyzewithkinesis/10.png)
**General**
- Enter Studio notebook name: **AnalyticsWorkshop-KDANotebook**
- Enter Runtime: **Apache Flink 1.15, Apache Zeppelin 0.10**
- Click Next
![pic](/anworkshopaws/images/6-analyzewithkinesis/11.png)
**IAM role**
- Select **Choose from IAM roles that Kinesis Data Analytics can assume**
- Select our previously created service role: **AnalyticsworkshopKinesisAnalyticsRole**
- In AWS Glue database, select: **analyticsworkshopdb**
- Click Next
![pic](/anworkshopaws/images/6-analyzewithkinesis/12.png)
**In Configurations**
- Parallelism: 4
- Parallelism per KPU: 1
- Uncheck the Turn on logging checkbox
- Skip everything else and go to the bottom, in tag: workshop: AnalyticsOnAWS
![pic](/anworkshopaws/images/6-analyzewithkinesis/13.png)

### Running the Kinesis Analytics Studio Notebook ###
Now that we have created our notebook, we can run it and try to execute some SQL queries.
- See our list of notebooks in Studio tab and click our newly created notebook AnalyticsWorkshop-KDANotebook
- Click Run
- Wait until the Status goes into Running mode. (It will take about 5-7 minutes)
![pic](/anworkshopaws/images/6-analyzewithkinesis/14.png)
- Click Open in Apache Zeppelin at the upper right hand side to open Zeppelin Notebook
- Click Create new note and give the note a name AnalyticsWorkshop-ZeppelinNote
- Paste this SQL query
 {{% notice note %}}
      %flink.ssql(type=update)
      SELECT * FROM raw_stream;
 {{% /notice %}}
![pic](/anworkshopaws/images/6-analyzewithkinesis/15.png)
- Click Add Paragraph
![pic](/anworkshopaws/images/6-analyzewithkinesis/16.png)
- Paste this SQL query
 {{% notice note %}}
      %flink.ssql(type=update)
      SELECT activity_type, count(*) as activity_cnt FROM raw_stream group by activity_type;
 {{% /notice %}}
![pic](/anworkshopaws/images/6-analyzewithkinesis/17.1.png)
- When all queries are pasted, Click the Play button at the top right of the paragraph
![pic](/anworkshopaws/images/6-analyzewithkinesis/18.png)
NOTE: There will be no data shown yet because this Streaming Analytics notebook processes streaming data. We have to send streaming data from our Kinesis Data Generator (if not already), so the notebook can display the result of the queries

### Generate Dummy Data to Kinesis Data Stream ###
To display data from queries run in Analytics Streaming notebook, we have to send the raw data from our Kinesis Data Generator.
- Go to the KinesisDataGeneratorURL. You can find this in the [Cloudformation stack's Output tab](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/outputs?filteringStatus=active&filteringText=&viewNested=true&hideStacks=false&stackId=arn%3Aaws%3Acloudformation%3Aus-east-1%3A860873776111%3Astack%2FKinesis-Data-Generator-Cognito-User%2Fe8fc0040-b69e-11ec-bd11-0e017e3a47f9).
![pic](/anworkshopaws/images/6-analyzewithkinesis/19.png)
- Login with your username & password
![pic](/anworkshopaws/images/6-analyzewithkinesis/20.png)
Fill this out:
- Region: us-east-1
- Stream/delivery stream: analytics-workshop-data-stream (DO NOT choose analytics-workshop-stream that you might have created in "Ingest and Store" module of this workshop)
- Ensure Records per second is Constant.
- Value for Records per second: 100 (DO NOT change this number for the workshop.)
- Ensure that Compress Records is unchecked.
- Record template (Template 1): In the big text area, insert the following json template:
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
- Then Click Send Data
![pic](/anworkshopaws/images/6-analyzewithkinesis/21.png)
- Go back to our Zeppelin Notebook. Wait for a few minutes, the result should be displayed
![pic](/anworkshopaws/images/6-analyzewithkinesis/22.png)