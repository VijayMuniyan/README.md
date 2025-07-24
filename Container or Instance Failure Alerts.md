Option 1: Set Up Container/Instance Failure Alerts Using AWS CloudWatch (For EC2 Instances)

Step 1: Set Up CloudWatch to Monitor EC2 Instance Metrics
     1.Enable EC2 CloudWatch Monitoring:
         1.CloudWatch provides basic metrics (CPU, disk, network) for EC2 instances.
         2.For enhanced monitoring, install the CloudWatch agent on your EC2 instances.

     To install CloudWatch Agent on an EC2 instance:
        1.SH into your EC2 instance.
        2.Run the following commands to install and configure the CloudWatch Agent:
        sudo yum install amazon-cloudwatch-agent -y
        sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
        3.Follow the wizard to configure the agent. You can choose to monitor additional metrics such as memory and disk.

  Step 2: Set Up CloudWatch Alarm for EC2 Instance Failures

  1.Go to the CloudWatch Console:
       1.Open the CloudWatch Console from the AWS Management Console.
  2.Create an Alarm:
       1.Navigate to Alarms > Create Alarm.
       2.Under Select metric, choose EC2 > Per-Instance Metrics.
       3.Select CPUUtilization, DiskReadOps, or StatusCheckFailed (or any other relevant metrics based on your needs).
  3.Set the Alarm Conditions:
       1.Set conditions to trigger when an EC2 instance goes down or fails certain thresholds.
       2.For example, StatusCheckFailed > 1 indicates the instance is in an unhealthy state.

  4.Set Notification:
       1.Under Actions, choose Create a new SNS topic (e.g., ec2-failure-alerts).
       2.Add your Tech Lead's email address for notifications.
       3.Click Create Alarm.

  Step 3: Test the Alarm
       1.To test the alarm, you can manually stop or force a failure on the EC2 instance, and the alarm will trigger if the failure condition is met.
       2.The Tech Lead will receive an email notification through SNS.
