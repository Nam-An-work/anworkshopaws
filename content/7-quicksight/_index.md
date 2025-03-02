---
title : "7. Visualize in Quicksight"
date : "2025-02-22"
weight : 3
chapter : false
---
![pic](/anworkshopaws/images/a-10.png) 
In this module, we are going use Amazon Quicksight to build a few visualizations over the data collected and stored in S3.
### Setting Up QuickSight ###
In this step we will visualize our processed data using QuickSight.
- Go to: [Quicksight Console](https://us-east-1.quicksight.aws.amazon.com/en/start)
- Click Sign up for QuickSight
- Ensure Enterprise is selected and click Continue
- QuickSight account name: yournameanalyticsworkshop
- Notification email address: you@youremail.com
- Select Amazon Athena - this enables QuickSight access to Amazon Athena databases
- Select Amazon S3
- Select naman-analytics-workshop-bucket
- Click Finish
Wait for your QuickSight account to be created
![pic](/anworkshopaws/images/7-quicksight/1.png)

### Adding a New Dataset ###
- Go to [quicksight](https://us-east-1.quicksight.aws.amazon.com/sn/start/data-sets)
- On top right, click New Analyze
- On top left, click New Dataset
![pic](/anworkshopaws/images/7-quicksight/2.png)
- Click Athena
**New Athena data source**
- Data source name: analyticsworkshop
- Click **Validate connection**
- Click **Create data source**
![pic](/anworkshopaws/images/7-quicksight/3.png)
**Choose your table**
- Database: contain sets of tables - select **analyticsworkshopdb**
- Tables: contain the data you can visualize - select **processed_data**
- Click Select
**Finish data set creation**
- Select **Directly query your data**
- Click **Visualize**
![pic](/anworkshopaws/images/7-quicksight/4.png)

### Using Amazon Quick Sight to Visualize Our Processed Data ###
On the bottom-left panel (Visual types):
- Hover on icons there to see names of the available visualizations
- Click on **Heat Map**
On top-left panel (Fields list)
- Click **device_id**
- Click **track_name**
Just above the visualization you should see Field wells: Rows: device_id | Columns: track_name
![pic](/anworkshopaws/images/7-quicksight/5.png)