---
title : "2.1. Create S3 Bucket"
date : "2025-02-22"
weight : 2
chapter : false
---
Navigate to S3 Console & Create a new bucket in us-east-1 region :
1. Go to [S3 Console](https://s3.console.aws.amazon.com/s3/home?region=us-east-1).
    - Click - **Create Bucket**.
        - Bucket Name - **yourname-analytics-workshop-bucket**
        ![pic](/anworkshopaws/images/2-ingestandstore/2.png)
        - Region - **US EAST (N. Virginia)**.
        - Optionally add Tags, e.g.:
            - workshop: AnalyticsOnAWS
            ![pic](/anworkshopaws/images/2-ingestandstore/2.1.png)
        - Click - **Create Bucket**.

2. Adding reference data
    - Open **yourname-analytics-workshop-bucket**
    ![pic](/anworkshopaws/images/2-ingestandstore/3.png)
        - Click - **Create folder**
        - New folder called - **data**
        - Click - **Create folder**
        ![pic](/anworkshopaws/images/2-ingestandstore/4.png)
    - Open - **data**
        - Click - **Create folder** (From inside the data folder)
        - New folder : **reference_data**
        ![pic](/anworkshopaws/images/2-ingestandstore/5.png)
        - Click - **Create folder**
        ![pic](/anworkshopaws/images/2-ingestandstore/6.png)
    - Open - **reference_data**
        - Download this file locally : [tracks_list.json](https://static.us-east-1.prod.workshops.aws/public/252b2158-4ee1-410c-b074-58190ec31cd6/static/data/tracks_list.json)
        - In the S3 Console, Click - **Upload**
            - Click Add files & upload the **tracks_list.json** file here
            ![pic](/anworkshopaws/images/2-ingestandstore/7.png)
            - Click Upload (bottom left)
            ![pic](/anworkshopaws/images/2-ingestandstore/8.png)