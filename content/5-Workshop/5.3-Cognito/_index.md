---
title: "Cognito Authentication"
date: 2024-01-01
weight: 3
pre: " <b> 5.3. </b> "
---

# Setting up Cognito Authentication

In this step, you will configure **Amazon Cognito** to manage user registration, authentication, and secure access tokens for host users. We will use the new application-centric Cognito console interface.

### 1. Create Amazon Cognito User Pool for Web Application (SPA)

1. **Open Amazon Cognito Console**
   * Access Amazon Cognito User Pools: [https://console.aws.amazon.com/cognito/v2/idp/user-pools](https://console.aws.amazon.com/cognito/v2/idp/user-pools)
   * Click **Create user pool** or **Get started for free in less than five minutes**.

2. **Define your application**
   * On the Define your application screen:
   * **Application type:** Select **Single-page application (SPA)**
   * Click **Next**.

3. **Name your application**
   * Enter the information:
   * **Application name:** `webquiz-dev-web-client`
   * Click **Next**.

4. **Configure options**
   * Set up sign-in options:
   * **Options for sign-in identifiers:** Select **Email**
   * **Required attributes for sign-up:** Keep default as `email`
   * Click **Next**.

5. **Add a return URL**
   * Enter the application's Callback URL: `http://localhost:3000/callback` *(If deploying to Production, replace with the actual application URL)*.
   * Click **Next**.

6. **Review and create**
   * Review your settings:
     * **Application type:** Single-page application (SPA)
     * **Application name:** `webquiz-dev-web-client`
     * **Sign-in identifier:** Email
     * **Callback URL:** `http://localhost:3000/callback`
   * Then click **Create your application**.

7. **Set up your application**
   * Upon successful creation, Amazon Cognito will automatically create:
     * User Pool
     * App Client (Public Client for SPA)
   * The Set up your application page will display sample code snippets (Quick setup guide) for integration.
   * If you don't need to integrate immediately, click **Go to overview** to navigate to the User Pool management page.

---

### 2. Configure Additional User Pool Settings

Once the User Pool is created, you must configure security policies:

1. Inside your User Pool details page, select the **Authentication methods** tab:
   * Verify the **Password policy** configuration:
     * Minimum length: `8` characters.
     * Check ✅ **Requires numbers**.
     * Check ✅ **Requires lowercase**.
     * Check ✅ **Requires uppercase**.
     * Uncheck ☐ **Requires symbols** (or keep checked if preferred).
2. Select the **Sign-up** tab:
   * **Self-service sign-up:** Check ✅ **Enabled** (to allow users to register themselves).
   * **Cognito-assisted verification:** Check ✅ **Enabled**.
   * **Verification method:** Select **Send email message**.
3. Select the **App clients** tab:
   * Click on the client named `webquiz-dev-web-client`.
   * Verify **Client secret** is set to **No client secret** (since it is a public SPA client).
   * Verify **Authentication flows** has:
     * ✅ `ALLOW_USER_SRP_AUTH`
     * ✅ `ALLOW_REFRESH_TOKEN_AUTH`

---

### 3. Record Cognito Details

Make sure you copy and save the following credentials from the **User Pool Overview** page:
*   **User Pool ID:** `ap-southeast-1_xxxxxxxxx`
*   **App Client ID** (found in the App Clients tab): `xxxxxxxxxxxxxxxxxxxxxxxxxx`

> [!IMPORTANT]
> You will need these two values when configuring the API Gateway authorizer and local frontend application variables later.
