---
title : "9. Warehouse on Redshift"
date : "2025-02-22"
weight : 3
chapter : false
---
![pic](/anworkshopaws/images/a-12.png) 
In this module, we are going to setup an Amazon Redshift cluster, and use AWS Glue to load the data into Amazon Redshift. We will learn about several design considerations and best practices on creating and loading data into tables in Redshift, and running queries against it.
### Create Redshift IAM Role ###
Go to: [Redshift Console](https://console.aws.amazon.com/iam/home?region=us-east-1#/roles)
- Click on **Create role**
- Select **Redshift** under Use case and Use cases for other AWS services:
- Select **Redshift - customizable**
- Click Next
![pic](/anworkshopaws/images/9-warehouseonredshift/1.png)
In Search box, search and check following two policies
- **AmazonS3FullAccess**
- **AWSGlueConsoleFullAccess**
- Click Next
- Give Role Name as **Analyticsworkshop_RedshiftRole**
![pic](/anworkshopaws/images/9-warehouseonredshift/2.png)
- Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
- Click **Create role**
![pic](/anworkshopaws/images/9-warehouseonredshift/3.png)

### Create Redshift cluster ###
Go to [Redshift cluster](https://console.aws.amazon.com/redshiftv2/home?region=us-east-1)
- Click **Provision** and manage clusters from menu panel on the left
- Click **Create Cluster**
- Leave **Cluster identifier** as redshift-cluster-1
- Select **dc2.large** as Node Type
- Select **Number of Nodes** as 2.
- Verify Configuration summary.
![pic](/anworkshopaws/images/9-warehouseonredshift/4.png)
- Change **Master user name** to **admin**
- Enter **Master user password**
![pic](/anworkshopaws/images/9-warehouseonredshift/5.png)
- Under **Cluster permissions**
- Click **Associate IAM role**
- Select previously created **Analyticsworkshop_RedshiftRole**
- Click **Associate IAM roles**
- **Analyticsworkshop_RedshiftRole** should appear under Associated IAM roles
![pic](/anworkshopaws/images/9-warehouseonredshift/6.png)
- Leave **Additional configurations** to default. It uses default VPC and default security group.
- Click **Create cluster**

### Create S3 Gateway Endpoint ###
Go to: [AWS VPC Console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Endpoints:)
- Click **Create endpoint**
- Name tag - optional: RedshiftS3EP
- Select **AWS Services** under **Service category** (which is the default selection)
![pic](/anworkshopaws/images/9-warehouseonredshift/7.png)
- Under **Service name** search box, search for **"s3"** and hit enter/return.
- **com.amazonaws.us-east-1.s3** should come up as search result. Select this option with type as Gateway
- Under **VPC**, chose default VPC. This is the same VPC which was used for configuring redshift cluster
![pic](/anworkshopaws/images/9-warehouseonredshift/8.png)
- Once you have double checked VPC id, move to configuring **Route tables** section.
- Select the listed route table (this should be the main route table). You can verify this by checking **Yes** under **Main** column.
- Leave **Policy** as default. (Full access)
![pic](/anworkshopaws/images/9-warehouseonredshift/9.png)
- Optionally add Tags, e.g.: * workshop: AnalyticsOnAWS
![pic](/anworkshopaws/images/9-warehouseonredshift/10.png)
- Click **Create endpoint**. It should take a couple of seconds to provision this. Once this is ready, you should see Status as - Available against the newly created S3 endpoint.

### Verify and add rules to the default security group ###
Go to: [VPC Security Groups](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#SecurityGroups:)
- Select the Redshift security group. It should be the default security if it was not changed during the Redshift cluster creation step.
![pic](/anworkshopaws/images/9-warehouseonredshift/11.png)
- Click **Edit inbound rules**
![pic](/anworkshopaws/images/9-warehouseonredshift/12.png)
- Click **Edit outbound rules**
![pic](/anworkshopaws/images/9-warehouseonredshift/13.png)

### Create Redshift connection under Glue Connection. ###
Go to: [Glue Connections Console](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/connectors)
- Under **Connections** Click **Create connection**
- Give **Connection name** as analytics_workshop.
- Choose **Connection** type as Amazon Redshift.
![pic](/anworkshopaws/images/9-warehouseonredshift/14.png)
Setup Connections access
- Choose **Database instances** as **redshift-cluster-1**
- Database name and Username should get populated automatically as **dev** and **admin** respectively.
- Password: Enter the password which you have used during Redshift cluster setup.
- Click **Create connection**
![pic](/anworkshopaws/images/9-warehouseonredshift/15.png)

### Create schema and redshift tables. ###
Go to: [Redshift Query Editor](https://us-east-1.console.aws.amazon.com/redshiftv2/home?region=us-east-1#query-editor:) 
- Click **Connect** to database
- **Connection**: Create a new connection
- **Authentication**: Temporary credentials
- **cluster: redshift**-cluster-1
- **Database name**: dev
- **Database user**: admin
- Click **Connect**
![pic](/anworkshopaws/images/9-warehouseonredshift/16.png)

- Execute the queries below to create schema and tables for raw and reference data.
{{% notice note %}}

            CREATE schema redshift_lab;
 {{% /notice %}}

{{% notice note %}}
            --    Create f_raw_1 table.
            CREATE TABLE IF not EXISTS redshift_lab.f_raw_1 (
            uuid            varchar(256),
            device_ts         timestamp,
            device_id        int,
            device_temp        int,
            track_id        int,
            activity_type        varchar(128),
            load_time        int
            );
 {{% /notice %}}

 {{% notice note %}}
            --    Create d_ref_data_1 table.
            CREATE TABLE IF NOT EXISTS redshift_lab.d_ref_data_1 (
            track_id        int,
            track_name    varchar(128),
            artist_name    varchar(128)
            );
 {{% /notice %}}
![pic](/anworkshopaws/images/9-warehouseonredshift/17.png)
### Use Jupyter Notebook in AWS Glue for interactive ETL development ###
Download and save this file locally on your laptop [file](https://static.us-east-1.prod.workshops.aws/public/252b2158-4ee1-410c-b074-58190ec31cd6/static/notebooks/analytics-workshop-redshift-glueis-notebook.ipynb)
- Go to: [Glue Studio jobs](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/jobs)
- Select **Jupyter Notebook** option
- Select **Upload and edit an existing notebook**
- Click **Choose file**
- Browse and upload **analytics-workshop-redshift-glueis-notebook.ipynb** which you downloaded earlier
- Click **Create**
![pic](/anworkshopaws/images/9-warehouseonredshift/18.png)
Under Notebook setup and Initial configuration
- Job name: AnalyticsOnAWS-Redshift
- IAM role Analyticsworkshop-GlueISRole
- Leave Kernel to default as Spark
- Click Start notebook
Execute the following queries to check the number of records in raw and reference data table.
 {{% notice note %}}
        select count(1) from redshift_lab.f_raw_1;

        select count(1) from redshift_lab.d_ref_data_1;
 {{% /notice %}}
  {{% notice note %}}
        select 
        track_name, 
        artist_name, 
        count(1) frequency
        from 
        redshift_lab.f_raw_1 fr
        inner join 
        redshift_lab.d_ref_data_1 drf
        on 
        fr.track_id = drf.track_id
        where 
        activity_type = 'Running'
        group by 
        track_name, artist_name
        order by 
        frequency desc
        limit 10;
 {{% /notice %}}