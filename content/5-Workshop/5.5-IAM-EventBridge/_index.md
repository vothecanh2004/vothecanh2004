---
title: "IAM Permissions & EventBridge"
date: 2024-01-01
weight: 5
pre: " <b> 5.5. </b> "
---

# IAM Permissions & EventBridge Setup

In this step, you will set up security and configuration policies for execution permissions and the asynchronous messaging backend bus.

---

### 1. Create Lambda Execution Role

First, create the IAM role that all Lambda functions will use to interact securely with AWS resources.

1. Open the **[Amazon IAM console](https://console.aws.amazon.com/iam/)**.
2. Select **Roles** on the left menu, then click **Create role**.
3. **Step 1 - Select trusted entity:**
   * **Trusted entity type:** Select **AWS service**.
   * **Service or use case:** Select **Lambda** from the dropdown.
   * Click **Next**.
4. **Step 2 - Add permissions:**
   * Search for and select the managed policy **`AWSLambdaBasicExecutionRole`** (this allows Lambdas to write execution logs to CloudWatch).
   * Click **Next**.
5. **Step 3 - Name, review, and create:**
   * **Role name:** Enter `webquiz-dev-lambda-role`.
   * Click **Create role**.

---

### 2. Attach Custom Inline Policy

Now, grant this role permission to access your specific DynamoDB tables, write events to EventBridge, and manage WebSocket connections.

1. Find and click on the newly created role `webquiz-dev-lambda-role`.
2. Under the **Permissions** tab, click **Add permissions** → Select **Create inline policy**.
3. Select the **JSON** tab and paste the following policy:
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "DynamoDBAccess",
               "Effect": "Allow",
               "Action": [
                   "dynamodb:GetItem",
                   "dynamodb:PutItem",
                   "dynamodb:UpdateItem",
                   "dynamodb:DeleteItem",
                   "dynamodb:Query",
                   "dynamodb:Scan",
                   "dynamodb:BatchGetItem",
                   "dynamodb:BatchWriteItem"
               ],
               "Resource": [
                   "arn:aws:dynamodb:us-east-1:*:table/webquiz-dev-*",
                   "arn:aws:dynamodb:us-east-1:*:table/webquiz-dev-*/index/*"
               ]
           },
           {
               "Sid": "EventBridgeAccess",
               "Effect": "Allow",
               "Action": [
                   "events:PutEvents"
               ],
               "Resource": "arn:aws:events:us-east-1:*:event-bus/webquiz-dev-game-events"
           },
           {
               "Sid": "WebSocketManagement",
               "Effect": "Allow",
               "Action": [
                   "execute-api:ManageConnections"
               ],
               "Resource": "arn:aws:execute-api:us-east-1:*:*/dev/*"
           }
       ]
   }
   ```
4. Click **Next**.
5. **Policy name:** Enter `webquiz-dev-lambda-policy`.
6. Click **Create policy**.

---

### 3. Create Custom EventBus in EventBridge

Create the EventBridge bus that will route live gameplay events (like answer submissions) asynchronously.

1. Open the **[Amazon EventBridge console](https://console.aws.amazon.com/events/)**.
2. Select **Event buses** on the left menu, then click **Create event bus**.
3. In the creation wizard:
   * **Name:** Enter `webquiz-dev-game-events`.
   * **Encryption:** Select **Use AWS owned key** (default).
   * Keep other settings blank/default.
   * Click **Create**.
