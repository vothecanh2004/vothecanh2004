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
![Image 1](/images/5-Workshop/5.5/5.5.1.png)
2. Select **Roles** on the left menu, then click **Create role**.
![Image 2](/images/5-Workshop/5.5/5.5.2.png)
3. **Step 1 - Select trusted entity:**
   * **Trusted entity type:** Select **AWS service**.
   * **Service or use case:** Select **Lambda** from the dropdown.
   * Click **Next**.
![Image 3](/images/5-Workshop/5.5/5.5.3.png)
4. **Step 2 - Add permissions:**
   * Search for and select the managed policy **`AWSLambdaBasicExecutionRole`** (this allows Lambdas to write execution logs to CloudWatch).
   * Click **Next**.
![Image 4](/images/5-Workshop/5.5/5.5.4.png)
5. **Step 3 - Name, review, and create:**
   * **Role name:** Enter `webquiz-dev-lambda-role`.
   * Click **Create role**.
![Image 5](/images/5-Workshop/5.5/5.5.5.png)

---

### 2. Attach Custom Inline Policy

Now, grant this role permission to access your specific DynamoDB tables, write events to EventBridge, and manage WebSocket connections.

1. Find and click on the newly created role `webquiz-dev-lambda-role`.
2. Under the **Permissions** tab, click **Add permissions** → Select **Create inline policy**.
3. Select the **JSON** tab and paste the following policy:
![Image 6](/images/5-Workshop/5.5/5.5.6.png)
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
                   "arn:aws:dynamodb:ap-southeast-1:*:table/webquiz-dev-*",
                   "arn:aws:dynamodb:ap-southeast-1:*:table/webquiz-dev-*/index/*"
               ]
           },
           {
               "Sid": "EventBridgeAccess",
               "Effect": "Allow",
               "Action": [
                   "events:PutEvents"
               ],
               "Resource": "arn:aws:events:ap-southeast-1:*:event-bus/webquiz-dev-game-events"
           },
           {
               "Sid": "WebSocketManagement",
               "Effect": "Allow",
               "Action": [
                   "execute-api:ManageConnections"
               ],
               "Resource": "arn:aws:execute-api:ap-southeast-1:*:*/dev/*"
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
![Image 7](/images/5-Workshop/5.5/5.5.7.png)
2. Select **Event buses** on the left menu, then click **Create event bus**.
![Image 8](/images/5-Workshop/5.5/5.5.8.png)
3. In the creation wizard:
   * **Name:** Enter `webquiz-dev-game-events`.
   * **Encryption:** Select **Use AWS owned key** (default).
   * Keep other settings blank/default.
   * Click **Create**.
![Image 9](/images/5-Workshop/5.5/5.5.9.png)
![Image 10](/images/5-Workshop/5.5/5.5.10.png)
