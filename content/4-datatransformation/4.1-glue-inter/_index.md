---
title : "4.1. Transform Data with AWS Glue (interactive sessions)"
date : "2025-02-22"
weight : 2
chapter : false
---
![pic](/anworkshopaws/images/a-04.png) 
1. Prepare IAM policies and role \
In this step you will navigate to IAM console and create the necessary IAM policies and role to work with AWS Glue Studio Jupyter notebooks and interactive sessions. Let's start by creating an IAM policy for the AWS Glue notebook role
    - Go to [Iam](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/policies)
    - Click **Policies** from menu panel on the left
    - Click **Create policy**
        - Click on **JSON** tab
        - Replace default text in policy editor window with the following policy statemenent.
        {{% notice note %}}
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": "iam:PassRole",
                    "Resource": "arn:aws:iam::180294200626:role/Analyticsworkshop-GlueISRole"
                },
                {
                    "Effect": "Allow",
                    "Action": [
                        "glue:*",
                        "s3:ListBucket",
                        "s3:GetObject",
                        "s3:PutObject",
                        "s3:DeleteObject",
                        "s3:GetBucketLocation",
                        "s3:ListAllMyBuckets",
                        "s3:GetLifecycleConfiguration",
                        "s3:GetBucketPolicy",
                        "s3:GetBucketCORS"
                    ],
                    "Resource": "*"
                }
            ]
        }
        {{% /notice %}}
{{% notice note %}}
Note that **Analyticsworkshop-GlueISRole** is the role that we create for the AWS Glue Studio Jupyter notebook in next step.
{{% /notice %}}
{{% notice warning %}}
Alert: Replace with your **AWS account ID** in the copied policy statement.
{{% /notice %}}
    ![pic](/anworkshopaws/images/4-datatransformation/1.png)
    - Click **Next** 
    - Policy Name: *AWSGlueInteractiveSessionPassRolePolicy*
    - Optionally write description for the policy:
        - Description: The policy allows AWS Glue notebook role to pass to interactive sessions so that the same role can be used in both places
    ![pic](/anworkshopaws/images/4-datatransformation/2.png)
    - Tags Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
    ![pic](/anworkshopaws/images/4-datatransformation/3.png)
Next, create an IAM role for AWS Glue notebook
- Go to [Iam](https://us-east-1.console.aws.amazon.com/iamv2/home#/roles)
    - Click **Roles** from menu panel on the left
    - Click **Create Role**
        - Choose the service that will use this role: **Glue** under Use Case and **Use cases for other AWS services**
        ![pic](/anworkshopaws/images/4-datatransformation/4.png)
        - Click Next
        - Search for following policies and select the checkbox against them:
        {{% notice note %}}
        AWSGlueServiceRole
        AwsGlueSessionUserRestrictedNotebookPolicy
        AWSGlueInteractiveSessionPassRolePolicy
        AmazonS3FullAccess
        {{% /notice %}}
    - Click **Next** 
    - Role name: *Analyticsworkshop-GlueISRole*
    ![pic](/anworkshopaws/images/4-datatransformation/5.png)
    - Make sure only four policies are attached to this role (**AWSGlueServiceRole, AwsGlueSessionUserRestrictedNotebookPolicy, AWSGlueInteractiveSessionPassRolePolicy, AmazonS3FullAccess**)
    - Optionally add Tags, e.g.: workshop: AnalyticsOnAWS
    - Click **Create role**
{{% notice note %}}
Note: We have granted full S3 access to the Glue role for the purpose of this workshop. It is recommended to grant only the permissions required to perform a task i.e. follow least-privilege permissions model in real/actual deployments. 
{{% /notice %}}

2. Use Jupyter Notebook in AWS Glue for interactive ETL development
In this step you will be creating an AWS Glue job with Jupyter Notebook to interactively develop Glue ETL scripts using PySpark.
    - Download and save this file locally on your laptop: [analytics-workshop-glueis-notebook.ipynb](https://static.us-east-1.prod.workshops.aws/public/252b2158-4ee1-410c-b074-58190ec31cd6/static/notebooks/analytics-workshop-glueis-notebook.ipynb)
    - Go to: [Glue Studio jobs](https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/jobs)
    - Select **Jupyter Notebook** option
        - Select **Upload and edit an existing notebook**
        - Click **Choose file**
        - Browse and upload **analytics-workshop-glueis-notebook.ipynb** which you downloaded earlier
        - Click **Create**
    ![pic](/anworkshopaws/images/4-datatransformation/6.png)
    - Under **Notebook setup** and **Initial configuration**
        - Job name: *AnalyticsOnAWS-GlueIS*
        - IAM role *Analyticsworkshop-GlueISRole*
        - Leave **Kernel** to default as Spark
        - Click **Start notebook**
Once the notebook is initialized, follow the instructions in the notebook

3. Validate - Transformed / Processed data has arrived in S3
Once the ETL script has ran successfully, return to the console: [Click me](https://s3.console.aws.amazon.com/s3/home?region=us-east-1)
    - Click - **naman-analytics-workshop-bucket > data**
    - Open the **processed-data** folder:
        - Ensure that .parquet files have been created in this folder.
Now that we have transformed the data, we can query the data using Amazon Athena. We could also further transform/aggregate the data with AWS Glue or Amazon EMR.
The next module on EMR is optional. You could skip it if you want to and proceed to Analyze with Athena.