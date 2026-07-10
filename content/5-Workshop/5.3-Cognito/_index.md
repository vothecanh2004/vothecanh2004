---
title: "Cognito Authentication"
date: 2024-01-01
weight: 3
pre: " <b> 5.3. </b> "
---

# Setting up Cognito Authentication

In this step, you will configure **Amazon Cognito** to manage user registration, authentication, and secure access tokens for host users. We will use the new application-centric Cognito console interface.

### 1. Create User Pool (New AWS Console Flow)

1. Open the **[Amazon Cognito console](https://console.aws.amazon.com/cognito/)**.
![Image 1](/images/5-Workshop/5-3-1.jpg)
2. Click **Create user pool**.
![Image 2](/images/5-Workshop/5-3-2.jpg)
3. **Step 1 - Define your application:**
   * **Application type:** Select **Single-page application (SPA)**.
   * Click **Next**.
![Image 3](/images/5-Workshop/5-3-3.jpg)
4. **Step 2 - Name your application:**
   * **Application name:** Enter `webquiz-dev-web-client`.
   * Click **Next**.
![Image 4](/images/5-Workshop/5-3-4.jpg)
5. **Step 3 - Configure options:**
   * **Options for sign-in identifiers:** Check ✅ **Email**.
   * **Required attributes for sign-up:** `email` (default).
   * Click **Next**.
![Image 5](/images/5-Workshop/5-3-5.jpg)
6. **Step 4 - Add a return URL:**
   * **Return URL:** Enter `http://localhost:3000/callback` (you will use this for local frontend integration).
   * Click **Next**.
![Image 6](/images/5-Workshop/5-3-6.jpg)
7. **Step 5 - Review and create:**
   * Review all configuration details.
   * Click **Create your application**.
8. **Step 6 - Set up your application:**
   * Cognito will display code integration examples.
   * Click **Go to overview** to access your User Pool.

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
*   **User Pool ID:** `us-east-1_xxxxxxxxx`
*   **App Client ID** (found in the App Clients tab): `xxxxxxxxxxxxxxxxxxxxxxxxxx`

> [!IMPORTANT]
> You will need these two values when configuring the API Gateway authorizer and local frontend application variables later.
