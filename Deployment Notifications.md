# Automated Email Notifications for Deployments in Jenkins

This guide will show you how to configure **Jenkins** to automatically send **email notifications** to the Tech Lead on **successful** or **failed** deployments using **Amazon SES** as the email service.

## Prerequisites

1. **Amazon SES** account with email address verification.
2. **Jenkins** installed and running.
3. The **Email Extension Plugin** installed in Jenkins.

## Steps

### Step 1: Set Up Amazon SES for Email Notifications

1. **Create Amazon SES Account**:
   - Sign in to **Amazon SES** and verify your Tech Lead's email address (e.g., `techlead@example.com`).
   - Note down the **SMTP username** and **SMTP password** for use in Jenkins.

2. **Set Up SES SMTP Settings**:
   - Use the SES SMTP endpoint (e.g., `email-smtp.us-east-1.amazonaws.com`) and port `587` for TLS.

### Step 2: Configure Jenkins to Send Email Notifications

1. **Install the Email Extension Plugin**:
   - Go to **Manage Jenkins** > **Manage Plugins** > **Available** > search for **Email Extension Plugin** and install it.

2. **Configure Email Settings**:
   - Go to **Manage Jenkins** > **Configure System**.
   - In the **Email Notification** section, set the **SMTP server** to the SES endpoint and enter your **SMTP username and password**.

3. **Configure Email Notifications in Jenkins Job**:
   - In your Jenkins job configuration, go to **Post-build Actions** > **Editable Email Notification**.
   - Set the **recipient list** to the Tech Lead's email address and configure the **subject** and **message** as desired.

### Step 3: Test the Email Notification Setup

1. Trigger a **manual build** in Jenkins to verify that the Tech Lead receives an email.
2. Ensure that notifications are sent on **both success and failure** by triggering a successful deployment and a failed one.

### Step 4: Customize the Email Content (Optional)

You can modify the **email body** to include more detailed information about the deployment (e.g., build logs, failure reasons).

Example:
```bash
Subject: Deployment Failure - ${PROJECT_NAME}

Hello Tech Lead,

The deployment for **${PROJECT_NAME}** failed. Please review the logs below:

- Build Status: ${BUILD_STATUS}
- Build Number: ${BUILD_NUMBER}
- Failure Details: ${BUILD_LOG}

