---
title: "Configure business logic computation for AWS Lambda functions"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

In this section, you will deploy the source code, configure environment variables, and test the connection for the **AWS Lambda** functions of the Chrono Genesis Game project. The following guide will walk you through 9 detailed steps to initialize and fully configure a Lambda function.

---

### Step 1: Initialize a new Lambda function
Navigate to the AWS Lambda service console on the AWS Management Console. Click the **Create function** button to start creating a new Lambda function.

**Technical Purpose:** Initialize the serverless computing environment for the Chrono Genesis Game project, which will contain the business logic code for the game (e.g., connecting, processing turns, ending matches, etc.).

![Create Lambda Function](/images/5-Workshop/overrall-lambda/1.%20Vao%20service%20Lambda%20-%20chon%20Create%20Lambda.png)

---

### Step 2: Configure basic settings and permissions (Execution Role)
On the "Create function" screen, perform the following:
1. Select **Author from scratch**.
2. Under **Function name**, enter the function name (e.g., `ConnectHandler`).
3. Expand the **Change default execution role** section, then select **Use an existing role**.
4. Choose the pre-configured Role for the project: `Chrono-lambda-execution-role`.
5. Click **Create function**.

**Technical Purpose:** Set up the name and permissions for the Lambda function through an IAM Role. Assigning the `Chrono-lambda-execution-role` ensures the Lambda has sufficient permissions to access DynamoDB, SQS, or API Gateway to serve the game's data flow.

![Configure name and Role for Lambda](/images/5-Workshop/overrall-lambda/2.%20dat%20ten%20lambda%20-%20enable%20custom%20executtion%20role%20-%20chon%20role%20Chrono-lambda-execution-role.png)

---

### Step 3: Prepare to upload the source code
Once the function is successfully created, the system redirects you to the function's details screen. Navigate to the **Code** tab. Under **Code source**, select **Upload from** and click on **.zip file**.

**Technical Purpose:** Prepare to deploy the game logic source code (which has been packaged, optimized using esbuild, and compressed into a .zip format) to the AWS Lambda runtime environment.

![Upload zip code file](/images/5-Workshop/overrall-lambda/3.%20Sau%20khi%20tao%20xong%2C%20upload%20file%20code%20lambda%20da%20esbuild%20va%20nen%20thanh%20file%20zip.png)

---

### Step 4: Launch the Upload dialog
When the **Upload a .zip file** dialog appears, click the **Upload** button to open the File Explorer on your personal computer.

**Technical Purpose:** Begin the process of selecting the source code archive (.zip) from your local machine to transfer it to the AWS cloud.

![Launch upload dialog](/images/5-Workshop/overrall-lambda/4.png)

---

### Step 5: Select the .zip file and save configuration
Browse to the folder containing the project's source code on your machine, select the `.zip` file corresponding to the logic of the current Lambda function (e.g., `ConnectHandler.zip`), then click **Save** to start the upload process.

**Technical Purpose:** Upload the carefully packaged executable source code to the AWS Lambda's direct storage repository, preparing for actual logic execution.

![Select zip file and save](/images/5-Workshop/overrall-lambda/5.png)

---

### Step 6: Verify successful source code upload
Observe the main screen of the Code tab. When the green notification **"Successfully updated the function..."** appears at the top, it confirms that your source code file has been successfully uploaded and deployed.

**Technical Purpose:** Ensure that the latest version of the source code has been safely overwritten into the Lambda function environment and is ready to operate.

![Upload successful](/images/5-Workshop/overrall-lambda/6.%20upload%20thanh%20cong.png)

---

### Step 7: Configure mock data (Test Event)
To ensure the Lambda processes functions correctly, switch to the **Test** tab on the interface. Here:
1. Select **Create new event**.
2. Enter the event name in **Event name** (e.g., `TestConnectEvent`).
3. Paste/format the JSON content in the **Event JSON** pane according to the expected input data payload standard of the game.
4. Click **Save**.

**Technical Purpose:** Configure a mock event (Test Event) to locally verify whether the newly uploaded source code logic works as designed before integrating it into the real API system.

![Configure Test Event](/images/5-Workshop/overrall-lambda/7.%20De%20dam%20bao%20lambda%20xu%20ly%20dung%20chuc%20nang%2C%20co%20the%20test%20truoc%20bang%20cach%20truy%20cap%20vao%20tab%20test%20event%20trong%20lambda.png)

---

### Step 8: Run the test and verify the results
Click the orange **Test** button to execute the function. Check the returned results in the **Execution result** pane.
If the screen shows a green **succeeded** status (e.g., with `statusCode: 200` and a successful body content), it proves your function operates correctly.

**Technical Purpose:** Independently verify that the Lambda function (e.g., ConnectHandler) successfully processes user requests without encountering network drops, logic errors, or missing system access permissions.

![Test successful result](/images/5-Workshop/overrall-lambda/8.%20Vi%20du%20test%20connectHandler%20thanh%20cong.png)

---

### Step 9: Verify the WebSocket connection Trigger
Navigate to the **Configuration** > **Triggers** tab. Check the current Trigger list.
Ensure that the trigger source is correctly linked to **API Gateway** and routed to the correct flow of the corresponding WebSocket API (e.g., `$connect`, `$disconnect`, or a specific Route).

**Technical Purpose:** Integrate the Lambda function into the actual network architecture. This action establishes the mandatory bridge for Amazon API Gateway to forward real-time events from players (via WebSocket connections) directly to the business logic handling Lambda function.

![Verify WebSocket Trigger](/images/5-Workshop/overrall-lambda/9.%20kiem%20tra%20Trigger%2C%20dam%20bao%20cac%20lambda%20cho%20websocket%20deu%20trigger%20vao%20api%20websocket%20gateway%20qua%20route.png)

---
