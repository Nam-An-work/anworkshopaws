---
title : "8. Serve with Lambda"
date : "2025-02-22"
weight : 3
chapter : false
---
![pic](/anworkshopaws/images/a-11.png) 
### Creating a Lambda Function ###
- Go to: [Lambda Console](https://console.aws.amazon.com/lambda/home?region=us-east-1)
- Click **Create function**
- Select **Author from scratch**
Under Basic Information
- Give Function name as Analyticsworkshop_top5Songs
- Select Runtime as Python 3.9
- Expand Choose or create an execution role under Permissions, make sure Create a new role with basic Lambda permissions is selected.
![pic](/anworkshopaws/images/8-servewithlambda/1.png) 
- Scroll down to Function Code section and replace existing code under in lambda_function.py with the python code below:
{{% notice note %}}
            import boto3
            import time
            import os

            # Environment Variables
            DATABASE = os.environ['DATABASE']
            TABLE = os.environ['TABLE']
            # Top X Constant
            TOPX = 5
            # S3 Constant
            S3_OUTPUT = f's3://{os.environ["BUCKET_NAME"]}/query_results/'
            # Number of Retries
            RETRY_COUNT = 10

            def lambda_handler(event, context):
                client = boto3.client('athena')
                # query variable with two environment variables and a constant
                query = f"""
                    SELECT track_name as \"Track Name\", 
                            artist_name as \"Artist Name\",
                            count(1) as \"Hits\" 
                    FROM {DATABASE}.{TABLE} 
                    GROUP BY 1,2 
                    ORDER BY 3 DESC
                    LIMIT {TOPX};
                """
                response = client.start_query_execution(
                    QueryString=query,
                    QueryExecutionContext={ 'Database': DATABASE },
                    ResultConfiguration={'OutputLocation': S3_OUTPUT}
                )
                query_execution_id = response['QueryExecutionId']
                # Get Execution Status
                for i in range(0, RETRY_COUNT):
                    # Get Query Execution
                    query_status = client.get_query_execution(
                        QueryExecutionId=query_execution_id
                    )
                    exec_status = query_status['QueryExecution']['Status']['State']
                    if exec_status == 'SUCCEEDED':
                        print(f'Status: {exec_status}')
                        break
                    elif exec_status == 'FAILED':
                        raise Exception(f'STATUS: {exec_status}')
                    else:
                        print(f'STATUS: {exec_status}')
                        time.sleep(i)
                else:
                    client.stop_query_execution(QueryExecutionId=query_execution_id)
                    raise Exception('TIME OVER')
                # Get Query Results
                result = client.get_query_results(QueryExecutionId=query_execution_id)
                print(result['ResultSet']['Rows'])
                # Function can return results to your application or service
                # return result['ResultSet']['Rows']
 {{% /notice %}}
![pic](/anworkshopaws/images/8-servewithlambda/2.png) 
### Environment Variables ###
Scroll down to Environment variables section and add below three Environment variables.
- Key: DATABASE, Value: analyticsworkshopdb
- Key: TABLE, Value: processed_data
- Key: BUCKET_NAME, Value: yourname-analytics-workshop-bucket
![pic](/anworkshopaws/images/8-servewithlambda/3.png) 
- Leave the **Memory (MB)** as default which is 128 MB
- Change **Timeout** to 10 seconds.
![pic](/anworkshopaws/images/8-servewithlambda/4.png) 
### Execution Role ###
- Select the **Permissions Tab** at the top:
    - Click the Role Name link under **Execution Role** to open the IAM Console in a new tab.
- Click **Add permissions** and click **Attach policies**
- Add the following two policies (search in filter box, check and hit Attach policy):
    - AmazonS3FullAccess
    - AmazonAthenaFullAccess
- Once these policies are attached to the role, close this tab.
![pic](/anworkshopaws/images/8-servewithlambda/5.png) 
### Configuring The Test Event ###
Our function is now ready to be tested. Deploy the function first by clicking on **Deploy** under the Function code section.
Next, let's configure a dummy test event to see execution results of our newly created lambda function.
Click Test on right top hand corner of the lambda console.
- A new window will pop up for us to configure test event.
    - Create new test event is selected by default.
    - Event name: Test
    - Template: Hello World
    - Leave everything as is
    - Click Save
    - Click Test again
You should be able to see the output in json format under Execution Result section.
![pic](/anworkshopaws/images/8-servewithlambda/6.png) 

### Verification through Athena ###
- Go to: [Athena Console](https://console.aws.amazon.com/athena/home?region=us-east-1#query)
- On the left panel, select **analyticsworkshopdb** from the dropdown
- Run the following query
{{% notice note %}}
        SELECT track_name as "Track Name",
            artist_name as "Artist Name",
            count(1) as "Hits" 
        FROM analyticsworkshopdb.processed_data 
        GROUP BY 1,2 
        ORDER BY 3 DESC 
        LIMIT 5;
 {{% /notice %}}
 ![pic](/anworkshopaws/images/8-servewithlambda/7.png) 