---
title : "5. Analyze with Athena"
date : "2025-02-22"
weight : 3
chapter : false
---
![pic](/anworkshopaws/images/a-08.png) 
In this step we will analyze the transformed data using Amazon Athena.
Login to the Amazon Athena Console.
- Go to: [Athena Console](https://console.aws.amazon.com/athena/home?region=us-east-1#query)
- As Athena uses the AWS Glue catalog for keeping track of data source, any S3 backed table in Glue will be visible to Athena.
- On the left panel, select 'analyticsworkshopdb' from the drop down
- Run the following query:
            {{% notice note %}}
        SELECT artist_name, count(artist_name) AS count
        FROM processed_data
        GROUP BY artist_name
        ORDER BY count desc
            {{% /notice %}}
- Explore the Athena UI and try running some queries. Try querying the emr_processed_data table.
- This query returns the list of tracks repeatedly played by devices. 
            {{% notice note %}}
        SELECT device_id,
        track_name,
        count(track_name) AS count
        FROM processed_data
        GROUP BY device_id, track_name
        ORDER BY count desc
            {{% /notice %}}
- Preview table
  ![pic](/anworkshopaws/images/5-analyzewithathena/1.png)