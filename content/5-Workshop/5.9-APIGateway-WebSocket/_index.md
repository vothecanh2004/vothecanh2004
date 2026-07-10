---
title: "API Gateway - WebSocket API"
date: 2024-01-01
weight: 9
pre: " <b> 5.9. </b> "
---

# Setting up API Gateway - WebSocket API

In this step, you will configure **Amazon API Gateway WebSocket API** to enable persistent bidirectional real-time connections between frontend clients and backend serverless layers.

---

### 1. Create WebSocket API

1. Open the **[Amazon API Gateway console](https://console.aws.amazon.com/apigateway/)**.
2. Click **Create API**.
![Image 1](/images/5-Workshop/5.9/5.9.1.png)
3. Under **Choose an API type**, find **WebSocket API** and click **Build**.
![Image 2](/images/5-Workshop/5.9/5.9.2.png)
4. Configure API details:
   * **API name:** Enter `webquiz-dev-websocket-api`.
   * **Route selection expression:** Enter `$request.body.action` (this configuration extracts the action name from incoming JSON payloads to route messages).
   * Click **Next**.
![Image 3](/images/5-Workshop/5.9/5.9.3.png)

---

### 2. Configure Routes

WebSocket uses specific routing keys. We will enable three default routes:

1. In the **Add routes** step:
   * Check ✅ **$connect**
   * Check ✅ **$disconnect**
   * Check ✅ **$default**
2. Click **Next**.

---

### 3. Attach Integrations

Connect each route path to its respective Lambda handler function:

1. Map the integrations:
   * For the **`$connect`** route:
     * **Integration type:** Select **Lambda**.
     * **Lambda function:** Select `webquiz-dev-ws-connect`.
   * For the **`$disconnect`** route:
     * **Integration type:** Select **Lambda**.
     * **Lambda function:** Select `webquiz-dev-ws-disconnect`.
   * For the **`$default`** route:
     * **Integration type:** Select **Lambda**.
     * **Lambda function:** Select `webquiz-dev-ws-message`.
![Image 4](/images/5-Workshop/5.9/5.9.4.png)
2. Click **Next**.

---

### 4. Create and Deploy Stage

1. **Step 4 - Add stages:**
   * **Stage name:** Enter `dev`.
   * Check ✅ **Auto-deploy**.
   * Click **Next**.
![Image 5](/images/5-Workshop/5.9/5.9.5.png)
2. Click **Create and deploy**.

---

### 5. Record the WebSocket URLs

Go to the **Stages** tab on the left and select stage **`dev`**. Record the two URLs:
![Image 6](/images/5-Workshop/5.9/5.9.6.png)

*   **WebSocket URL:**
    ```
    wss://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/dev
    ```
    *(Used by the frontend React application to connect).*
*   **Connections URL:**
    ```
    https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/dev
    ```
    *(Used by backend Lambda functions to push real-time broadcasts).*

---

### 6. Update Lambda Environment Variables

Now that the WebSocket API is deployed, you must update the environment variables for your Lambda functions to allow them to broadcast messages.

1. Open the **AWS Lambda console**.
2. Update the **`webquiz-dev-ws-message`** function:
   * Open the function details → **Configuration** tab → **Environment variables**.
   * Click **Edit** and set:
     * `WEBSOCKET_ENDPOINT` = `https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/dev` (the **Connections URL** with `https://` prefix, **not** `wss://`).
   * Click **Save**.
![Image 7](/images/5-Workshop/5.9/5.9.7.png)
3. Update the **`webquiz-dev-score-calculator`** function:
   * Open the function details → **Configuration** tab → **Environment variables**.
   * Click **Edit** and set:
     * `WEBSOCKET_ENDPOINT` = `https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/dev`
   * Click **Save**.
![Image 8](/images/5-Workshop/5.9/5.9.8.png)
