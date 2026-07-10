---
title: "DynamoDB Tables"
date: 2024-01-01
weight: 4
pre: " <b> 5.4. </b> "
---

# Designing & Creating DynamoDB Tables

In this step, you will deploy the database layer. Instead of a traditional SQL database or Redis, **WebQuiz** uses **Amazon DynamoDB**—a serverless NoSQL database. We will configure all tables in **On-Demand** capacity mode to optimize cost.

---

### 1. Create the Database Tables

Follow the steps below to create all 7 DynamoDB tables:

#### 1.1 Users Table (`webquiz-dev-users`)
1. Open the **[Amazon DynamoDB console](https://console.aws.amazon.com/dynamodb/)**.
2. Select **Tables** on the left menu, then click **Create table**.
![Image 1](/images/5-Workshop/5.4/5.4.1.png)
3. Fill in the **Table details**:
   * **Table name:** `webquiz-dev-users`
   * **Partition key:** `userId` | **String**
   * Keep **Sort key** empty.
4. Under **Table settings**, select **Customize settings**.
![Image 2](/images/5-Workshop/5.4/5.4.2.png)
5. Set **Capacity mode** to **On-demand**.
6. In **Secondary indexes**, click **Create global index**:
   * **Partition key:** `email` | **String**
   * **Index name:** `email-index`
   * **Attribute projections:** **All**
   * Click **Create index**.
![Image 3](/images/5-Workshop/5.4/5.4.3.png)
7. Keep encryption defaults and click **Create table**.
![Image 4](/images/5-Workshop/5.4/5.4.4.png)

#### 1.2 Quizzes Table (`webquiz-dev-quizzes`)
1. Click **Create table**:
   * **Table name:** `webquiz-dev-quizzes`
   * **Partition key:** `quizId` | **String**
2. Select **Customize settings** → **On-demand** capacity mode.
![Image 5](/images/5-Workshop/5.4/5.4.5.png)
3. In **Secondary indexes**, click **Create global index**:
   * **Partition key:** `userId` | **String**
   * **Sort key:** `createdAt` | **String**
   * **Index name:** `userId-createdAt-index`
   * **Attribute projections:** **All**
   * Click **Create index**.
![Image 6](/images/5-Workshop/5.4/5.4.6.png)
4. Click **Create table**.
![Image 7](/images/5-Workshop/5.4/5.4.7.png)

#### 1.3 Questions Table (`webquiz-dev-questions`)
1. Click **Create table**:
   * **Table name:** `webquiz-dev-questions`
   * **Partition key:** `quizId` | **String**
   * **Sort key:** Check box and enter `questionId` | **String**
2. Select **Customize settings** → **On-demand** capacity mode.
![Image 8](/images/5-Workshop/5.4/5.4.8.png)
3. Click **Create table**.

#### 1.4 Rooms Table (`webquiz-dev-rooms`)
1. Click **Create table**:
   * **Table name:** `webquiz-dev-rooms`
   * **Partition key:** `roomPin` | **String**
2. Select **Customize settings** → **On-demand** capacity mode.
![Image 9](/images/5-Workshop/5.4/5.4.9.png)
3. In **Secondary indexes**, click **Create global index** (create 2 GSIs):
   * **GSI 1:**
     * **Partition key:** `hostId` | **String**
     * **Sort key:** `createdAt` | **String**
     * **Index name:** `hostId-createdAt-index`
     * **Attribute projections:** **All**
     * Click **Create index**.
![Image 10](/images/5-Workshop/5.4/5.4.10.png)
   * **GSI 2:**
     * **Partition key:** `status` | **String**
     * **Sort key:** `createdAt` | **String**
     * **Index name:** `status-createdAt-index`
     * **Attribute projections:** **Keys only**
     * Click **Create index**.
![Image 11](/images/5-Workshop/5.4/5.4.11.png)
4. Click **Create table**.
![Image 12](/images/5-Workshop/5.4/5.4.12.png)
5. Once created, select `webquiz-dev-rooms` table, open **Additional settings** tab:
   * Under **Time to Live (TTL)**, click **Turn on**:
     * **TTL attribute name:** Enter `ttl`.
     * Click **Turn on TTL**.
![Image 13](/images/5-Workshop/5.4/5.4.13.png)

#### 1.5 Connections Table (`webquiz-dev-connections`)
1. Click **Create table**:
   * **Table name:** `webquiz-dev-connections`
   * **Partition key:** `connectionId` | **String**
2. Select **Customize settings** → **On-demand** capacity mode.
![Image 14](/images/5-Workshop/5.4/5.4.14.png)
3. In **Secondary indexes**, click **Create global index**:
   * **Partition key:** `roomPin` | **String**
   * **Index name:** `roomPin-index`
   * **Attribute projections:** **All**
   * Click **Create index**.
![Image 15](/images/5-Workshop/5.4/5.4.15.png)
4. Click **Create table**.
![Image 16](/images/5-Workshop/5.4/5.4.16.png)
5. After creation, open the **Additional settings** tab of the `webquiz-dev-connections` table:
   * Turn on **Time to Live (TTL)** with attribute name `ttl`.
![Image 17](/images/5-Workshop/5.4/5.4.17.png)

#### 1.6 Game State Table (`webquiz-dev-game-state`)
This table manages all transient gameplay metrics (current player scores, submitted answers, and lobby connections).
1. Click **Create table**:
   * **Table name:** `webquiz-dev-game-state`
   * **Partition key:** `pk` | **String**
   * **Sort key:** Check box and enter `sk` | **String**
2. Select **Customize settings** → **On-demand** capacity mode.
![Image 18](/images/5-Workshop/5.4/5.4.18.png)
3. In **Secondary indexes**, click **Create global index**:
   * **Partition key:** `gsi1pk` | **String**
   * **Sort key:** `gsi1sk` | **String**
   * **Index name:** `gsi1`
   * **Attribute projections:** **All**
   * Click **Create index**.
![Image 19](/images/5-Workshop/5.4/5.4.19.png)
4. Click **Create table**.
![Image 20](/images/5-Workshop/5.4/5.4.20.png)
5. Enable **Time to Live (TTL)** on the table with attribute name `ttl`.

#### 1.7 Game Results Table (`webquiz-dev-game-results`)
1. Click **Create table**:
   * **Table name:** `webquiz-dev-game-results`
   * **Partition key:** `roomPin` | **String**
   * **Sort key:** Check box and enter `orderId` | **String**
2. Select **Customize settings** → **On-demand** capacity mode.
![Image 21](/images/5-Workshop/5.4/5.4.21.png)
3. Click **Create table**.
![Image 22](/images/5-Workshop/5.4/5.4.22.png)

---

### 2. Database Schema Summary

Below is a summary of the database schema created:

| Table Name | Partition Key | Sort Key | Global Secondary Index (GSI) | TTL Enabled |
| :--- | :--- | :--- | :--- | :--- |
| **webquiz-dev-users** | `userId` (S) | - | `email-index` (PK: `email`) | No |
| **webquiz-dev-quizzes** | `quizId` (S) | - | `userId-createdAt-index` (PK: `userId`, SK: `createdAt`) | No |
| **webquiz-dev-questions** | `quizId` (S) | `questionId` (S) | - | No |
| **webquiz-dev-rooms** | `roomPin` (S) | - | `hostId-createdAt-index`, `status-createdAt-index` | Yes (`ttl`) |
| **webquiz-dev-connections** | `connectionId` (S) | - | `roomPin-index` (PK: `roomPin`) | Yes (`ttl`) |
| **webquiz-dev-game-state** | `pk` (S) | `sk` (S) | `gsi1` (PK: `gsi1pk`, SK: `gsi1sk`) | Yes (`ttl`) |
| **webquiz-dev-game-results** | `roomPin` (S) | `orderId` (S) | - | No |
