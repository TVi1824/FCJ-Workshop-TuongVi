---
title : "Deploy Logic with AWS Lambda & SQS"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

In this section, you will deploy the source code, configure environment variables, and test the connection for the **ConnectHandler-function** on AWS Lambda.

---

### 1. Deploy and Configure the Lambda Function Source Code

1. In the detail view of the **ConnectHandler-function** Lambda function, navigate to the **Code** tab. Locate the **Upload from** button and select the **.zip file** option.
![Upload zip file for Lambda function](/images/5-Workshop/5.5-Policy/10.png)
![Upload zip file for Lambda function](/images/5-Workshop/5.5-Policy/806.png)

2. A popup window will appear. Click **Upload** (or **Choose file**) and select the pre-packaged `index.zip` file from your local machine. Then click **Open** to upload the code.

![Upload zip file for Lambda function](/images/5-Workshop/5.5-Policy/55.png)

3. Wait a few seconds. The system will display a green banner — **"Successfully updated the function ConnectHandler-function"** — confirming the upload was successful.
![Upload zip file for Lambda function](/images/5-Workshop/5.5-Policy/818.png)

4. Switch to the **Configuration** tab, then select **Environment variables** from the left-hand menu.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/153.png)

5. Click the **Edit** button and then select **Add environment variable**.

6. Fill in the following information to configure a secure connection to the Database:

   | Key | Value |
   |-----|-------|
   | `DB_SECRET_NAME` | `ChronoGame/PartnerDB/Credentials` |

7. Click **Save** to apply the configuration.

![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/850.png)

8. Switch to the **Test** tab. Under **Test event**, select **Create new event** and name the event `Test-connection`. Then click **Save**.

9. Click the orange **Test** button to invoke the Lambda function.

10. Expand the **Execution results (Details)** section to review the output. If the function returns `statusCode: 200` and `body: "Connected successfully."` along with a green **"Executing function: succeeded"** banner, your configuration is complete and working correctly.

![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/936.png)

---

### 2. Configure the Lambda PostMatchWorker

**Step 1:** In the detail view of the **Lambda PostMatchWorker-function**, switch to the **Configuration** tab, select **Environment variables**, and click **Edit** to configure the database connection environment variables (such as `DB_ACCESS_KEY_ID`, `DB_REGION`, `DB_SECRET_ACCESS_KEY`).
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/postMatch.png)

**Step 2:** Switch to the **Test** tab. Under **Test event**, select **Create new event** (or **Edit saved event**), name the event **test-postMatchWorker**, paste a JSON snippet that simulates an SQS message into the **Event JSON** field, then click **Save**.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/723.png)

**Step 3:** Click the orange **Test** button to invoke the function. Then, expand the **Execution results (Details)** section: if you see a green **"Executing function: succeeded"** banner along with `statusCode: 200` and `body: "Post match processing complete."`, your processing pipeline is working correctly.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/641.png)

---

### 3. Configure the Lambda ProcessGameEngine

**Step 1:** In the detail view of the **Lambda ProcessGameEngine-function**, go to the **Code** tab and update the source code by uploading a **.zip** file. Once the upload completes, the system will display a green **"Successfully updated the function ProcessGameEngine-function"** confirmation banner.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/842.png)

**Step 2:** Navigate to the test configuration (usually the **Test** tab), create a new event, and set the **Event name** to **Test-EndTurn**. In the **Event JSON** field, paste a JSON snippet that simulates an API Gateway request (containing fields such as `connectionId` and a `body` with the player's `END_TURN` action). Click **Save** to prepare for the test run.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/708.png)

### 4. Repeat for the Remaining Lambda Functions
- Lambda SaveDeck, StartMatch, HandleTimeout.

---

**Step 1:** Navigate to the **Amazon SQS** service and click **Create queue**. Under **Details**, select **Standard** as the queue type and enter **Match-Result-queue** in the **Name** field. Leave all other **Configuration** settings at their defaults.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/246.png)

**Step 2:** Scroll down to the **Encryption** section. Ensure Server-side encryption is set to **Enabled** with the key type **Amazon SQS key (SSE-SQS)**.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/131.png)

**Step 3:** Under the **Access policy** section, select the **Basic** method and configure the default send/receive message permissions to **Only the queue owner**.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/108.png)

**Step 4:** Scroll to the bottom of the page, leave the **Redrive allow policy** and **Dead-letter queue** settings as **Disabled**, then proceed to create the queue.
![Configure Environment Variables for Lambda](/images/5-Workshop/5.5-Policy/313.png)

**Step 5:** Navigate to the **Lambda PostMatchWorker-function** detail view. On the **Configuration** tab, select **Triggers** from the left-hand menu and click **Add trigger**. In the dropdown, select **SQS** as the source, find and select the **Match-Result-queue** you just created, set the **Batch size** to **10**, check **Activate trigger**, then click **Add**.
![Add SQS Trigger to Lambda](/images/5-Workshop/5.5-Policy/351.png)

**Step 6:** Return to the **Configuration** tab. You will see the newly added **SQS trigger** listed with a **Creating** status. Wait a few minutes for AWS to establish the connection — the status will then switch to active.
![Add SQS Trigger to Lambda](/images/5-Workshop/5.5-Policy/324.png)
