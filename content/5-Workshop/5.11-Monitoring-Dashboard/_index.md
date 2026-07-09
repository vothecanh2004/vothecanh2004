---
title: "CloudWatch Monitoring & Dashboard"
date: 2024-01-01
weight: 11
pre: " <b> 5.11. </b> "
---

# CloudWatch Monitoring & Dashboard Setup

In this step, you will finalize the asynchronous event routing by setting up **Amazon EventBridge Rules** (which require target Lambda functions created in previous steps) and construct **Amazon CloudWatch** alarms and dashboards to monitor your application's health.

---

### 1. Configure EventBridge Rules (Routing Events to Targets)

Now that the target Lambdas are deployed, configure the EventBridge rules to automatically trigger them when specific events land on the bus.

#### 1.1 Create Rule for AnswerSubmitted
1. Open the **[Amazon EventBridge console](https://console.aws.amazon.com/events/)**.
2. Select **Rules** on the left menu.
3. Choose the event bus `webquiz-dev-game-events` from the dropdown.
4. Click **Create rule**.
5. **Step 1 - Define rule detail:**
   * **Name:** `webquiz-dev-answer-submitted`
   * **Description:** `Trigger score calculator when answer is submitted`.
   * **Event bus:** `webquiz-dev-game-events`.
   * **Rule type:** Select **Rule with an event pattern**.
   * Click **Next**.
6. **Step 2 - Build event pattern:**
   * **Event source:** Select **Other**.
   * **Creation method:** Select **Custom pattern (JSON editor)**.
   * **Event pattern:**
     ```json
     {
       "source": ["quiz.game"],
       "detail-type": ["AnswerSubmitted"]
     }
     ```
   * Click **Next**.
7. **Step 3 - Select targets:**
   * **Target type:** **AWS service**.
   * **Select a target:** **Lambda function**.
   * **Function:** Select `webquiz-dev-score-calculator`.
   * Click **Next**.
8. **Step 4 - Review and create:**
   * Click **Create rule**.

#### 1.2 Create Rule for GameEnded
1. Under the `webquiz-dev-game-events` bus, click **Create rule**.
2. **Step 1 - Define rule detail:**
   * **Name:** `webquiz-dev-game-ended`.
   * **Event bus:** `webquiz-dev-game-events`.
   * **Rule type:** **Rule with an event pattern**.
3. **Step 2 - Build event pattern:**
   * **Event source:** **Other**.
   * **Event pattern:**
     ```json
     {
       "source": ["quiz.game"],
       "detail-type": ["GameEnded"]
     }
     ```
4. **Step 3 - Select targets:**
   * **Target:** **Lambda function** → Select `webquiz-dev-game-results-saver`.
5. Click **Create rule**.

---

### 2. Configure CloudWatch Alarms

Set up alerts to notify you of execution failures, excessive WebSocket connection loads, or database throttling.

1. Open the **[Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/)**.
2. Select **Alarms** on the left menu, then click **All alarms** → **Create alarm**.

#### 2.1 Lambda Errors Alarm
*   **Metric:** Select `AWS/Lambda` → `Errors` → Sum.
*   **Period:** `5 minutes`.
*   **Threshold:** Greater than `5`.
*   **Name:** `webquiz-dev-lambda-errors`.

#### 2.2 WebSocket Connection Alarm
*   **Metric:** Select `AWS/ApiGateway` → `ConnectCount` (Dimension: ApiId = your WebSocket API ID).
*   **Period:** `1 minute`.
*   **Threshold:** Greater than `10000` (to track abnormal connection spike traffic).
*   **Name:** `webquiz-dev-websocket-connections`.

#### 2.3 DynamoDB Throttle Alarm
*   **Metric:** Select `AWS/DynamoDB` → `ThrottledRequests` (Dimension: TableName = `webquiz-dev-game-state`).
*   **Period:** `1 minute`.
*   **Threshold:** Greater than `1`.
*   **Name:** `webquiz-dev-dynamodb-throttle`.

---

### 3. Create CloudWatch Dashboard

Build a unified operational dashboard to visualize metrics.

1. Select **Dashboards** on the left menu of the CloudWatch console.
2. Click **Create dashboard**.
3. **Dashboard name:** Enter `webquiz-dev-dashboard`.
4. Add the following widgets:
   * **Widget 1 (Lambda Invocations):** Metric widget showing `Invocations` for `webquiz-dev-ws-message`, `webquiz-dev-score-calculator`, and `webquiz-dev-quiz-crud`.
   * **Widget 2 (Lambda Durations):** Metric widget showing `Duration` (p99 stat) for `webquiz-dev-ws-message` and `webquiz-dev-score-calculator`.
   * **Widget 3 (WebSocket Metrics):** Metric widget showing `ConnectCount` and `MessageCount` for your WebSocket API.
   * **Widget 4 (DynamoDB Consumed Capacity):** Metric widget showing `ConsumedReadCapacityUnits`, `ConsumedWriteCapacityUnits`, and `ThrottledRequests` for `webquiz-dev-game-state`.
5. Click **Save dashboard**.
