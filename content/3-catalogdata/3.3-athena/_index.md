---
title : "3.3. Query ingested data using Amazon Athena"
date : "2025-02-22"
weight : 2
chapter : false
---
Let's query the newly ingested data using Amazon Athena
1. Go to [AWS Athena](https://us-east-1.console.aws.amazon.com/athena/home?region=us-east-1#query)
   - If necessary, click **Edit seetings** in the blue alert near the top of the Athena console
   ![pic](/anworkshopaws/images/3-catalogdata/13.png)
   - Click **Editor** tab
   - On the left panel (**Database**) drop down , select **analyticsworkshopdb** > select table **raw**
   - Click on **3 dots** (3 vertical dots) > Select **Preview Table**
   - Review the output
   - In query editor, paste the following query:
            {{% notice note %}}
            SELECT activity_type, count(activity_type)
            FROM raw
            GROUP BY  activity_type
            ORDER BY  activity_type
            {{% /notice %}}
   - Click on **Run**
   ![pic](/anworkshopaws/images/3-catalogdata/14.png)