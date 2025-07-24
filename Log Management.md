Step 1: Set Up AWS S3 to Store Application Logs

1.Create an S3 Bucket:

   1.Open the AWS Console and go to S3.
   2.Click Create bucket.
   3.Choose a unique name for the bucket (e.g., my-app-logs), select your preferred region, and click Create bucket.
   
2.Upload Application Logs to S3:
  1.You can manually upload logs or use a script to send logs to the bucket.
  
To upload using the AWS CLI:
aws s3 cp /path/to/logfile.log s3://my-app-logs/


Step 2: Set Up AWS CloudWatch to Stream Logs from S3
CloudWatch will act as the intermediate step to stream logs from S3 to Grafana.
1.Create a CloudWatch Log Group:
  1.Go to CloudWatch in the AWS Console.
  2.Select Logs > Create log group.
  3.Name your log group (e.g., my-app-log-group) and click Create.

2.Create a CloudWatch Log Stream:
  1.Inside your log group, click Create log stream.
  2.Name it (e.g., my-app-log-stream) and click Create.

3.Set Up Lambda Function to Transfer Logs from S3 to CloudWatch:

  1.Go to the Lambda Console in AWS and click Create function.
  2.Select Author from scratch, name the function (e.g., s3-to-cloudwatch), and select Python as the runtime.
  3.Add permissions for Lambda to read from S3 and write to CloudWatch Logs by assigning an appropriate IAM role.

  Lambda Python Code Example:

  import boto3
import json
import time

cloudwatch_client = boto3.client('logs')
s3_client = boto3.client('s3')

def lambda_handler(event, context):
    log_group_name = 'my-app-log-group'
    log_stream_name = 'my-app-log-stream'

    for record in event['Records']:
        s3_bucket = record['s3']['bucket']['name']
        s3_key = record['s3']['object']['key']
        response = s3_client.get_object(Bucket=s3_bucket, Key=s3_key)
        log_data = response['Body'].read().decode('utf-8')

        # Send log data to CloudWatch
        cloudwatch_client.put_log_events(
            logGroupName=log_group_name,
            logStreamName=log_stream_name,
            logEvents=[{
                'timestamp': int(round(time.time() * 1000)),
                'message': log_data
            }]
        )

    return {
        'statusCode': 200,
        'body': json.dumps('Log Data Processed')
    }
Set the Lambda function to trigger whenever new objects are uploaded to your S3 bucket.

4.Configure S3 Event Notification:
   1.In S3, navigate to Properties > Event notifications.
   2.Create a notification to trigger the Lambda function when a new object (log file) is uploaded to the S3 bucket.


Step 3: Set Up Grafana to Visualize CloudWatch Logs
   1.Install Grafana (if not installed):
   For Ubuntu, run:
   sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update
sudo apt-get install grafana
sudo systemctl enable grafana-server
sudo systemctl start grafana-server

  2.Access Grafana:
  Open your browser and navigate to http://<your-server-ip>:3000.
  Default login: admin/admin (you will be prompted to change the password).


  3.Add CloudWatch as a Data Source:
     1.In Grafana, click Configuration (gear icon) on the left sidebar and select Data Sources.
     2.Click Add data source, then choose CloudWatch.
     3.Enter your AWS credentials (Access Key ID and Secret Access Key) and specify the AWS region where your logs are stored.
     4.Click Save & Test to confirm the connection.


  Step 4: Create a Grafana Dashboard to Visualize Logs
  1.Create a New Dashboard:
     1.In Grafana, go to Create > Dashboard.
     2.Click Add Panel.

  2.Configure the Panel to Display CloudWatch Logs:
     1.Select CloudWatch as the Data Source.
     2.Select the log group (my-app-log-group) and log stream (my-app-log-stream) you created earlier.
     3.Customize the visualization, for example, display logs as a table or time-series graph.

  3.Save the Dashboard:
     1.Give your dashboard a name (e.g., My App Logs Dashboard).
     2.Click Save.


Step 5: Automate and Test the Real-Time Log Streaming
     1.Upload a new log file to your S3 bucket (either manually or through your application).
     2.Once uploaded, the Lambda function will trigger and push the logs to CloudWatch.
     3.Grafana will automatically pick up the new logs and update the dashboard.


