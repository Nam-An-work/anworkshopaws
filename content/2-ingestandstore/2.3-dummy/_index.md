---
title : "2.3. Generate Dummy Data"
date : "2025-02-22"
weight : 2
chapter : false
---
In this step we will configure Kinesis Data Generator to produce fake data and ingest it into Kinesis Firehose. 
1. Configure Amazon Cognito for Kinesis Data Generator - In this step we will launch a cloud formation stack that will configure Cognito. This cloudformation scripts launches in N.Virginia region (No need to change this region)
    - Go to AWS [Cloudformation](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=Kinesis-Data-Generator-Cognito-User&templateURL=https://aws-kdg-tools-us-east-1.s3.amazonaws.com/cognito-setup.yaml)
    - Click - **Next**
    ![pic](/anworkshopaws/images/2-ingestandstore/18.png)
    - Specify Details:
        - Username: admin
            - Password: **choose an alphanumeric password**
            - Click **Next**
    ![pic](/anworkshopaws/images/2-ingestandstore/19.png)
    - Options:
        - Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
        - Click - **Next**
    ![pic](/anworkshopaws/images/2-ingestandstore/20.png)
    - Scroll down
        - I acknowledge that AWS CloudFormation might create IAM resources: **Check**
        ![pic](/anworkshopaws/images/2-ingestandstore/21.png)
    - Click **Next and Submit**
    - Refresh your AWS Cloudformation Console
    - Wait till the stack status changes to Create_Complete
    - Select the Kinesis-Data-Generator-Cognito-User stack
    ![pic](/anworkshopaws/images/2-ingestandstore/22.png)
    - Go to outputs tab: click on the link that says: **KinesisDataGeneratorUrl** - This will open your Kinesis Data Generator tool
    ![pic](/anworkshopaws/images/2-ingestandstore/23.png)
2. On Amazon Kinesis Data Generator homepage
    - Login with your username & password from previous step
    ![pic](/anworkshopaws/images/2-ingestandstore/24.png)
    - Region: **us-east-1**
    - Stream/delivery stream: **analytics-workshop-stream**
    - Records per second: **2000**
    ![pic](/anworkshopaws/images/2-ingestandstore/25.png)
    - Record template (Template 1): In the **big text area**, insert the following json template:
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
    - Click - **Send Data**
Once the tool sends ~10,000 messages, you can click on - **Stop sending data to Kinesis**

3. Validate that data has arrived in S3
- After few moments go to the [S3 console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)
- Navigate to: **naman-analytics-workshop-bucket > data**
- There should be a folder called **raw** > Open it and keep navigating, you will notice that firehose has dumped the data in S3 using **yyyy/mm/dd/hh** partitioning
![pic](/anworkshopaws/images/2-ingestandstore/27.png)

If you have received the dummy data in your s3 buckets, we are good to proceed to next step!