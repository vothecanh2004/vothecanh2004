---
title : "Prerequisites"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Prerequisites

To successfully complete this hands-on workshop to build the **WebQuiz** realtime interactive platform, you need to prepare your local development environment and configure the appropriate AWS account permissions.

---

#### 1. IAM Permissions

You need to use an IAM User or IAM Role with appropriate permissions to clean up and operate the resources in this workshop.

{{% notice note %}}
For the most convenient personal lab practice and to avoid permission errors (Access Denied), we recommend using **AdministratorAccess** permissions for your lab account.
{{% /notice %}}

If you want to configure a least privilege policy, follow these steps to create an IAM Policy on the AWS Console:

1. Sign in to the **AWS Management Console** and navigate to the **IAM** service.
2. Select **Policies** on the left navigation bar, then click **Create policy**.
3. In the policy creation interface, switch to the **JSON** editor, clear any default template code, and paste the following JSON policy block:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DynamoDBCleanup",
      "Effect": "Allow",
      "Action": [
        "dynamodb:DeleteTable",
        "dynamodb:DescribeTable",
        "dynamodb:ListTables"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CognitoCleanup",
      "Effect": "Allow",
      "Action": [
        "cognito-idp:DeleteUserPool",
        "cognito-idp:DescribeUserPool",
        "cognito-idp:DeleteUserPoolClient"
      ],
      "Resource": "*"
    },
    {
      "Sid": "S3Cleanup",
      "Effect": "Allow",
      "Action": [
        "s3:DeleteBucket",
        "s3:DeleteObject",
        "s3:DeleteObjectVersion",
        "s3:ListBucket",
        "s3:ListBucketVersions",
        "s3:GetBucketLocation"
      ],
      "Resource": "*"
    },
    {
      "Sid": "APIGatewayCleanup",
      "Effect": "Allow",
      "Action": [
        "apigateway:DELETE",
        "apigateway:GET"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudFrontCleanup",
      "Effect": "Allow",
      "Action": [
        "cloudfront:DeleteDistribution",
        "cloudfront:GetDistribution",
        "cloudfront:UpdateDistribution"
      ],
      "Resource": "*"
    },
    {
      "Sid": "LambdaCleanup",
      "Effect": "Allow",
      "Action": [
        "lambda:DeleteFunction",
        "lambda:GetFunction",
        "lambda:RemovePermission"
      ],
      "Resource": "*"
    },
    {
      "Sid": "EventBridgeCleanup",
      "Effect": "Allow",
      "Action": [
        "events:DeleteEventBus",
        "events:DeleteRule",
        "events:RemoveTargets",
        "events:DescribeRule",
        "events:DescribeEventBus"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchCleanup",
      "Effect": "Allow",
      "Action": [
        "cloudwatch:DeleteAlarms",
        "cloudwatch:DescribeAlarms",
        "logs:DeleteLogGroup",
        "logs:DescribeLogGroups"
      ],
      "Resource": "*"
    },
    {
      "Sid": "IAMCleanup",
      "Effect": "Allow",
      "Action": [
        "iam:DeleteRole",
        "iam:DeleteRolePolicy",
        "iam:DetachRolePolicy",
        "iam:ListRolePolicies",
        "iam:ListAttachedRolePolicies",
        "iam:GetRole"
      ],
      "Resource": "*"
    }
  ]
}
```

5. Click **Next**.
6. Enter a name for the policy (e.g., `WebQuizCleanupPolicy`), add an optional description, and click **Create policy**.
7. Attach this policy to your **IAM User** (navigate to **Users** -> select your User -> **Add permissions** -> **Attach policies directly** and select the `WebQuizCleanupPolicy` you just created).

---

#### 2. Local Workspace Setup

Ensure your workstation has the following tools installed and configured:

##### A. AWS CLI
*   Install AWS CLI and configure your IAM user credentials:
    ```bash
    aws configure
    ```
    Ensure you specify a default region (such as `ap-southeast-1`).

##### B. Node.js & npm
*   The WebQuiz Lambda backend and frontend application are written in Node.js. You need Node.js version **18.x** or **20.x** (LTS).
*   Verify your installation:
    ```bash
    node -v
    npm -v
    ```

##### C. Git & wscat
*   Used to fetch the project source code, and `wscat` for WebSocket API testing.
    ```bash
    git --version
    npm install -g wscat
    ```

---

#### 3. Download Project Source Code

The WebQuiz source code is structured into two main directories:
1.  **webquiz-frontend/**: Static React UI for the host dashboard, waiting room, and live game interaction.
2.  **webquiz-backend/** (Lambda Backend): Node.js Lambda code for REST APIs (create room, join), WebSockets (real-time communication), and EventBridge workers (scoring, saving results).

Clone the project from your workshop repository (replace the URL with your actual repo if applicable):
```bash
git clone https://github.com/USER/webquiz-workshop.git
cd webquiz-workshop
```

---

#### 4. Choose Infrastructure Deployment Method

In this workshop, you can choose one of the following two methods to build your serverless infrastructure:

*   **Option A (Recommended - Fast): Automate deployment using AWS CloudFormation**
    *   The infrastructure will be automatically initialized.
    *   The following chapters will serve as guides for you to **Verify and Validate** the created resources rather than recreating them.
*   **Option B: Manually configure step-by-step (Manual)**
    *   You will **skip** Step 4 (running CloudFormation) below.
    *   Starting from the next chapter, you will manually follow the instructions on the AWS Console to build the Serverless architecture from scratch.

---

#### Instructions for Option A: Automatic Deployment using CloudFormation

To prepare the environment quickly, deploy the provided CloudFormation template.

{{% notice info %}}
**Note on Deployment Scope**: 
If you choose automatic deployment via CloudFormation, the system will automatically initialize the basic resources including DynamoDB, Cognito, S3, CloudFront, Lambda, API Gateway, and EventBridge. You can **skip the manual creation steps** and only focus on **verifying configurations**, updating environment variables, uploading Frontend code to S3, and testing the real-time WebSocket flow.
{{% /notice %}}

1. The browser will open the CloudFormation Console with the configuration pre-filled (using the provided `architecture.yaml` file).
2. On the **Specify stack details** screen, verify the parameters and click **Next**.
3. Scroll to the bottom of the Review page, check **I acknowledge that AWS CloudFormation might create IAM resources with custom names.** and click **Submit** to start deployment.
4. Wait for the deployment process to complete (Status changes to `CREATE_COMPLETE`).

After the CloudFormation Stack deploys successfully, most of your backend is ready! In the upcoming labs, you will configure the Frontend, deploy static assets to S3, and perform end-to-end testing of the WebSocket real-time game flows and EventBridge architecture.
