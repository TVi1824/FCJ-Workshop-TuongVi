---
title: "Prerequisites & Cognito"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2. </b> "
---

## Goals

Before deploying the application, you need to prepare the required AWS resources and configure the authentication service. In this section, you will create the necessary IAM roles and permissions for AWS services, then set up an Amazon Cognito User Pool for user authentication.

These configurations provide the foundation for the backend deployment in the following sections.

### Why complete this section first?

- **AWS Permissions:** Configure the required IAM Roles and policies so that AWS services such as Lambda and Amazon SQS can interact securely.
- **User Authentication:** Create an Amazon Cognito User Pool to manage user registration and authentication. The **User Pool ID**, **Client ID**, and **Client Secret** obtained in this section will be required when configuring the backend application.
- **Deployment Preparation:** Completing these prerequisites ensures that all required AWS resources are available before deploying the application.

## Content

1. [IAM Configuration](5.2.1-iam/)
2. [Amazon Cognito Setup](5.2.2-cognito/)
