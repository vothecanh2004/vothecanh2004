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
2. Click **Create API**.
3. Under **Choose an API type**, find **HTTP API** and click **Build**.
4. **Step 1 - Create and configure integrations:**
   * **API name:** Enter `webquiz-dev-http-api`.
   * **IP address type:** Select **IPv4**.
   * *(Do not add integrations at this step)*.
   * Click **Next**.
5. **Step 2 - Configure routes:**
   * Click **Next** (we will configure routes manually later).
6. **Step 3 - Define stages:**
   * **Stage name:** Enter `dev`.
   * Check ✅ **Auto-deploy**.
   * Click **Next**.
7. **Step 4 - Review and create:**
   * Click **Create**.

---

### 2. Configure CORS

To allow frontend requests from different domains or local hosts:

1. Inside your HTTP API detail menu on the left, select **CORS**.
2. Click **Configure** and add the following settings:
   * **Access-Control-Allow-Origin:** Enter `*`.
   * **Access-Control-Allow-Headers:** Enter `Content-Type, Authorization`.
   * **Access-Control-Allow-Methods:** Enter `GET, POST, PUT, DELETE, OPTIONS`.
   * **Access-Control-Max-Age:** Enter `300`.
   * Click **Save**.

---

### 3. Create Cognito JWT Authorizer

1. Select **Authorization** on the left menu.
2. Under the **Manage authorizers** tab, click **Create**.
3. Configure the Authorizer:
   * **Authorizer type:** Select **JWT**.
   * **Name:** Enter `CognitoAuthorizer`.
   * **Identity source:** Enter `$request.header.Authorization`.
   * **Issuer URL:** Enter `https://cognito-idp.us-east-1.amazonaws.com/YOUR_USER_POOL_ID` (replace `YOUR_USER_POOL_ID` with the actual User Pool ID from Step 3).
   * **Audience:** Enter the **App Client ID** (copied from Step 3).
   * Click **Create**.

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
3. Click **Attach authorizer** (if applicable) and select `CognitoAuthorizer`.

---

### 6. Record the REST Endpoint URL

1. Go to **Stages** on the left menu.
2. Click on the stage named **`dev`**.
3. Copy and save the **Invoke URL**:
   ```
   https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/dev
   ```
   *(You will need this REST endpoint URL for frontend configuration).*
