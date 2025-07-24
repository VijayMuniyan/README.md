# API Failure Alerting - AWS Setup

This guide explains how to configure **API failure alerting** to send email notifications to the Tech Lead whenever there is an API failure (e.g., HTTP 500, 403, etc.) using **AWS CloudWatch**, **SNS**, and **CloudWatch Alarms**.

## Prerequisites

- **AWS Account** with permissions to create CloudWatch, SNS, and Lambda resources.
- **API Gateway** or custom API server that logs HTTP responses.

## Steps to Configure API Failure Alerting

### Step 1: Set Up Amazon CloudWatch for Monitoring API Responses

1. **Create a Log Group** in **CloudWatch**:
   - Go to **CloudWatch Console** > **Logs** > **Create log group** (e.g., `api-log-group`).

2. **Send API Logs to CloudWatch**:
   - Ensure your API service is logging HTTP response statuses to **CloudWatch Logs**. 
   - For **API Gateway**, enable **CloudWatch Logs** in your API settings.
   - For custom servers, use the AWS SDK to send logs to CloudWatch.

### Step 2: Create an Amazon SNS Topic for Email Notifications

1. **Create an SNS Topic**:
   - Go to **Amazon SNS Console** > **Create topic** and name it `api-failure-alerts`.

2. **Subscribe to the SNS Topic**:
   - Create a subscription and select **Email** as the protocol. Enter the Tech Lead's email address and click **Create subscription**.
   - Confirm the subscription via the email sent.

### Step 3: Create CloudWatch Metric Filter for API Failures

1. Go to **CloudWatch Console** > **Logs**.
2. Choose **Log Group** (`api-log-group`) and click **Create Metric Filter**.
3. Enter a filter pattern to capture HTTP errors (e.g., `500` or `403`).

### Step 4: Set Up CloudWatch Alarm to Monitor the Metric

1. Go to **CloudWatch Console** > **Alarms** > **Create Alarm**.
2. Select the metric you created and configure the alarm conditions.
3. Set up an action to send notifications to the SNS topic (`api-failure-alerts`).

### Step 5: Test the Alarm

1. **Simulate an API failure** by logging a `500` or `403` error.
2. Ensure the Tech Lead receives an **email alert** from SNS when the alarm is triggered.

## Conclusion

You have successfully configured **API failure alerting** to notify the **Tech Lead** via email when a failure occurs in your API.

## Troubleshooting

- **No email received**: Ensure the SNS subscription is confirmed.
- **No alarm triggered**: Double-check that the correct logs are being captured in CloudWatch and the filter is properly set.

