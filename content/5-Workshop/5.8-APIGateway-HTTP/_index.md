---
title: "API Gateway - HTTP API"
date: 2024-01-01
weight: 8
pre: " <b> 5.8. </b> "
---

# Setting up API Gateway - HTTP API

In this step, you will configure **Amazon API Gateway HTTP API** to route REST requests to their respective backend Lambda functions. We will also secure the protected endpoints using our Cognito JWT Authorizer.

---

### 1. Create HTTP API

1. Open the **[Amazon API Gateway console](https://console.aws.amazon.com/apigateway/)**.
![Image 1](/images/5-Workshop/5.8/5.8.1.png)
2. Click **Create API**.
![Image 2](/images/5-Workshop/5.8/5.8.2.png)
3. Under **Choose an API type**, find **HTTP API** and click **Build**.
4. **Step 1 - Create and configure integrations:**
   * **API name:** Enter `webquiz-dev-http-api`.
   * **IP address type:** Select **IPv4**.
   * *(Do not add integrations at this step)*.
   * Click **Next**.
5. **Step 2 - Configure routes:**
   * Click **Next** (we will configure routes manually later).
![Image 4](/images/5-Workshop/5.8/5.8.4.png)
6. **Step 3 - Define stages:**
   * **Stage name:** Enter `dev`.
   * Check ✅ **Auto-deploy**.
   * Click **Next**.
![Image 5](/images/5-Workshop/5.8/5.8.5.png)
7. **Step 4 - Review and create:**
   * Click **Create**.
![Image 6](/images/5-Workshop/5.8/5.8.6.png)
![Image 7](/images/5-Workshop/5.8/5.8.7.png)

---

### 2. Configure CORS

To allow frontend requests from different domains or local hosts:

1. Inside your HTTP API detail menu on the left, select **CORS**.
![Image 8](/images/5-Workshop/5.8/5.8.8.png)
2. Click **Configure** and add the following settings:
   * **Access-Control-Allow-Origin:** Enter `*`.
   * **Access-Control-Allow-Headers:** Enter `Content-Type, Authorization`.
   * **Access-Control-Allow-Methods:** Enter `GET, POST, PUT, DELETE, OPTIONS`.
   * **Access-Control-Max-Age:** Enter `300`.
   * Click **Save**.
![Image 9](/images/5-Workshop/5.8/5.8.9.png)
![Image 10](/images/5-Workshop/5.8/5.8.10.png)

---

### 3. Create Cognito JWT Authorizer

1. Select **Authorization** on the left menu.
![Image 11](/images/5-Workshop/5.8/5.8.11.png)
2. Under the **Manage authorizers** tab, click **Create**.
![Image 12](/images/5-Workshop/5.8/5.8.12.png)
3. Configure the Authorizer:
   * **Authorizer type:** Select **JWT**.
   * **Name:** Enter `CognitoAuthorizer`.
   * **Identity source:** Enter `$request.header.Authorization`.
   * **Issuer URL:** Enter `https://cognito-idp.ap-southeast-1.amazonaws.com/YOUR_USER_POOL_ID` (replace `YOUR_USER_POOL_ID` with the actual User Pool ID from Step 3).
![Image 13](/images/5-Workshop/5.8/5.8.13.png)
   * **Audience:** Enter the **App Client ID** (copied from Step 3).
![Image 14](/images/5-Workshop/5.8/5.8.14.png)
   * Click **Create**.
![Image 15](/images/5-Workshop/5.8/5.8.15.png)

---

### 4. Create Integrations

Connect the HTTP API Gateway to the backend Lambda functions:

1. Select **Integrations** on the left menu.
2. Click **Create** for two integrations:
   * **Integration 1 (Quiz CRUD):**
     * **Integration target:** Select **Lambda function**.
     * **Lambda function:** Select `webquiz-dev-quiz-crud`.
     * Click **Create**.
   * **Integration 2 (Room Management):**
     * Click **Create**.
     * **Integration target:** Select **Lambda function**.
     * **Lambda function:** Select `webquiz-dev-room-management`.
     * Click **Create**.

---

### 5. Create Routes & Attach Permissions

Now, map the API paths, attach their integrations, and configure authorization:

1. Select **Routes** on the left menu.
![Image 21](/images/5-Workshop/5.8/5.8.21.png)
2. Create and configure the following 5 routes:

| Route Route | Method | Path | Integration Target | Authorizer Type |
| :--- | :--- | :--- | :--- | :--- |
| **Route 1** | `ANY` | `/quizzes` | `webquiz-dev-quiz-crud` | `CognitoAuthorizer` |
| **Route 2** | `ANY` | `/quizzes/{proxy+}` | `webquiz-dev-quiz-crud` | `CognitoAuthorizer` |
| **Route 3** | `POST` | `/rooms` | `webquiz-dev-room-management` | `CognitoAuthorizer` |
| **Route 4** | `GET` | `/rooms/{pin}` | `webquiz-dev-room-management` | **None** (Public) |
| **Route 5** | `POST` | `/rooms/{pin}/join` | `webquiz-dev-room-management` | **None** (Public) |

*How to configure a route:*
1. Click **Create** and enter the **Method** and **Path**.
2. Click on the route you just created → Click **Attach integration** and select the correct target function.
![Image 24](/images/5-Workshop/5.8/5.8.24.png)
![Image 25](/images/5-Workshop/5.8/5.8.25.png)
![Image 26](/images/5-Workshop/5.8/5.8.26.png)
![Image 27](/images/5-Workshop/5.8/5.8.27.png)
![Image 28](/images/5-Workshop/5.8/5.8.28.png)
![Image 29](/images/5-Workshop/5.8/5.8.29.png)
![Image 30](/images/5-Workshop/5.8/5.8.30.png)
![Image 31](/images/5-Workshop/5.8/5.8.31.png)
![Image 32](/images/5-Workshop/5.8/5.8.32.png)
3. Click **Attach authorizer** (if applicable) and select `CognitoAuthorizer`.
![Image 33](/images/5-Workshop/5.8/5.8.33.png)
![Image 34](/images/5-Workshop/5.8/5.8.34.png)
![Image 35](/images/5-Workshop/5.8/5.8.35.png)

---

### 6. Record the REST Endpoint URL

1. Go to **Stages** on the left menu.
2. Click on the stage named **`dev`**.
3. Copy and save the **Invoke URL**:
   ```
   https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com/dev
   ```
   *(You will need this REST endpoint URL for frontend configuration).*
