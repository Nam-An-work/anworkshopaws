---
title : "6.3. Create Glue Catalog Table"
date : "2025-02-22"
weight : 2
chapter : false
---
Our Kinesis Application Notebook will get information about the data source from AWS Glue. When you create your Studio notebook, you specify the AWS Glue database that contains your connection information. When you access your data sources, you specify AWS Glue tables contained in the database.
- Go to [AWS glue](https://console.aws.amazon.com/glue/home?region=us-east-1)
- From the left sidebar, go to Databases and click our previously created database **analyticsworkshopdb**
- Click **Tables in analyticsworkshopdb**
- Click **Add tables** drop down menu and then select **Add table manually** \

**In Table Properties**
- Enter Table Name: **raw_stream**
![pic](/anworkshopaws/images/6-analyzewithkinesis/5.png)

**In Data store**
- Select the type of source: Kinesis
- Skip Select a kinesis data stream. (Stream in my account should be selected by default)
- Region: US East (N. Virginia) us-east-1
- Kinesis stream name: analytics-workshop-data-stream
- Skip sample size
![pic](/anworkshopaws/images/6-analyzewithkinesis/6.png)

**In Data format**
- Classification: JSON
- Then Click Next
![pic](/anworkshopaws/images/6-analyzewithkinesis/7.png)

**In Schema**
- Click Edit Schema as JSON and insert the following json text:
      {{% notice note %}}
      [{
         "Name": "uuid",
         "Type": "string",
         "Comment": ""
      },
      {
         "Name": "device_ts",
         "Type": "timestamp",
         "Comment": ""
      },
      {
         "Name": "device_id",
         "Type": "int",
         "Comment": ""
      },
      {
         "Name": "device_temp",
         "Type": "int",
         "Comment": ""
      },
      {
         "Name": "track_id",
         "Type": "int",
         "Comment": ""
      },
      {
         "Name": "activity_type",
         "Type": "string",
         "Comment": ""
      }]
      {{% /notice %}}
![pic](/anworkshopaws/images/6-analyzewithkinesis/8.png)
- Then Click Next

**Skip Partition Indices** \
**Review and create**

- Verify that the new Glue table **raw_stream** is properly created. Refresh the table list if it has not shown up yet.
- Click the newly created table **raw_stream**
- Click **Actions** and click **Edit table**
   - Under Table properties, add new property:
   - Key: kinesisanalytics.proctime
   - Value: proctime
![pic](/anworkshopaws/images/6-analyzewithkinesis/9.png)