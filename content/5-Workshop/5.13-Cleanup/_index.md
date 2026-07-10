---
title: "Resource Cleanup"
date: 2024-01-01
weight: 13
chapter: false
pre: " <b> 5.13. </b> "
---

# Resource Cleanup

To avoid incurring unexpected charges on your AWS account, make sure to delete all the resources created during this workshop.

Follow this sequence to safely clean up your environment:

---

### 1. Delete Frontend Assets

#### 1.1 Delete S3 Bucket Objects
1. Open the **[Amazon S3 console](https://console.aws.amazon.com/s3/)**.
2. Select your bucket `webquiz-dev-frontend-YOUR_ACCOUNT_ID`.
3. Click **Empty** to delete all objects inside the bucket.
4. Once the bucket is empty, click **Delete**, type the name of the bucket to confirm, and delete it.

#### 1.2 Delete CloudFront Distribution
1. Open the **[Amazon CloudFront console](https://console.aws.amazon.com/cloudfront/)**.
2. Locate your distribution, select it, and click **Disable**.
3. Wait for the status to change to `Disabled` (a few minutes).
4. Select the distribution again and click **Delete**.

---

### 2. Delete API Gateway Endpoints

1. Open the **[Amazon API Gateway console](https://console.aws.amazon.com/apigateway/)**.
2. Locate the HTTP API `webquiz-dev-http-api`, click **Actions** (or click the three dots) → select **Delete**.
3. Locate the WebSocket API `webquiz-dev-websocket-api`, click **Actions** → select **Delete**.

---

### 3. Delete Lambda Functions & IAM Permissions

1. Open the **[AWS Lambda console](https://console.aws.amazon.com/lambda/)**.
2. Under **Functions**, search for functions beginning with `webquiz-dev-`.
3. Select all 7 functions:
   * `webquiz-dev-quiz-crud`
   * `webquiz-dev-room-management`
   * `webquiz-dev-ws-connect`
   * `webquiz-dev-ws-disconnect`
   * `webquiz-dev-ws-message`
   * `webquiz-dev-score-calculator`
   * `webquiz-dev-game-results-saver`
4. Click **Actions** → **Delete**.
5. Open the **[Amazon IAM console](https://console.aws.amazon.com/iam/)**:
   * Under **Roles**, search for `webquiz-dev-lambda-role` and delete it.

---

### 4. Delete Database & Event Messaging Layer

1. Open the **[Amazon DynamoDB console](https://console.aws.amazon.com/dynamodb/)**.
2. Under **Tables**, select all 7 tables starting with `webquiz-dev-`:
   * `webquiz-dev-users`
   * `webquiz-dev-quizzes`
   * `webquiz-dev-questions`
   * `webquiz-dev-rooms`
   * `webquiz-dev-connections`
   * `webquiz-dev-game-state`
   * `webquiz-dev-game-results`
3. Click **Delete table**, type `delete` to confirm, and click **Delete**.
4. Open the **[Amazon EventBridge console](https://console.aws.amazon.com/events/)**:
   * Select **Event buses**, click on your bus `webquiz-dev-game-events` and delete it.

---

### 5. Delete Cognito User Pool

1. Open the **[Amazon Cognito console](https://console.aws.amazon.com/cognito/)**.
2. Locate and select your User Pool `webquiz-dev-users`.
3. Click **Delete**, type the User Pool ID to confirm, and delete it.
