# 🚀 Deploy Web Application using AWS Serverless Services

This project demonstrates how to deploy a **serverless web application** using various AWS services including:

- IAM
- AWS Lambda
- Amazon API Gateway
- Amazon DynamoDB
- AWS Certificate Manager (ACM)
- Amazon Route 53

Instead of managing EC2 instances, this project uses **AWS Serverless Architecture**, where Lambda executes backend code, API Gateway exposes REST APIs, DynamoDB stores application data, ACM provides SSL certificates, and Route 53 manages DNS routing.

---

# 🏗️ AWS Architecture

```text
                     User
                       │
                       │
                       ▼
               learnwith-ankit.com
                       │
                       │
                 Amazon Route53
                       │
                       ▼
              API Gateway (REST API)
                       │
                       ▼
                AWS Lambda Function
                       │
                       ▼
              Amazon DynamoDB Table
```

---

# 📚 AWS Services Used

- AWS IAM
- AWS Lambda
- Amazon API Gateway
- Amazon DynamoDB
- AWS Certificate Manager (ACM)
- Amazon Route 53

---

# 📋 Prerequisites

Before starting this project, make sure you have:

- AWS Account
- Registered Domain Name
- Route53 Hosted Zone
- Basic knowledge of AWS
- Python Application ZIP File

---

# 🚀 Phase 1 : Create IAM Role

The Lambda function requires permissions to:

- Write CloudWatch Logs
- Read and Write DynamoDB

Therefore, first create an IAM Role.

## Step 1

Navigate to

```
AWS Console
→ IAM
→ Roles
→ Create Role
```

---

## Step 2

Select

```
Trusted Entity Type - AWS Service
```

Use Case

```
Lambda
```

Click

```
Next
```

---

## Step 3

Attach the following IAM Policies

```
AWSLambdaBasicExecutionRole
AmazonDynamoDBFullAccess
```

These permissions allow Lambda to

- Write CloudWatch Logs
- Access DynamoDB Table

---

## Step 4

Provide Role Details

Role Name

```
lambda-with-dynamodb
```

Click

```
Create Role
```

✅ IAM Role created successfully.

---

# 🚀 Phase 2 : Create Lambda Function

Now create the backend Lambda Function.

Navigate to

```
AWS Console
→ Lambda
→ Create Function
```

---

## Step 1

Choose

```
Author from Scratch
```

Function Name

```
serverless-api
```

Runtime

```
Python 3.14
```

Architecture

```
x86_64
```

---

## Step 2

Expand

```
Change default execution role
```

Choose

```
Use an existing role
```

Select

```
lambda-with-dynamodb
```

Click

```
Create Function
```

---

# Upload Application Code

After creating the Lambda function,

Open

```
Code Tab
```

Click

```
Upload From
```

Choose

```
.zip file
```

Upload

```
serverless-project.zip
```

Click

```
Save
```

> **Note:** The ZIP file contains the complete backend application code including the Lambda handler and HTML pages.

---

# Verify Lambda

Once uploaded successfully, your Lambda should contain files similar to:

```
serverless-project/
│
├── lambda_function.py
├── contactus.html
├── success.html
└── requirements.txt
```

Click

```
Deploy
```

Lambda is now ready.

---

# 🚀 Phase 3 : Create Amazon DynamoDB Table

Amazon DynamoDB is a fully managed NoSQL database service used to store the contact form details submitted by users.

In this project, every form submission (Name, Email, and Description) will be stored inside a DynamoDB table.

---

## Step 1

Navigate to

```
AWS Console
→ DynamoDB
→ Tables
→ Create Table
```

---

## Step 2

Provide the following details
```
Table Name    - ankittable
Partition Key - email
Data Type     - String

```

> **Note:** The Partition Key uniquely identifies each item in the table. In this project, we are using the user's email as the primary key.

---

## Step 3

Table Settings

Select

```
Default Settings
```

AWS will automatically configure:

- On-demand Capacity Mode
- Encryption at Rest
- Point-in-Time Recovery (Optional)
- Auto Scaling (Managed by AWS)

You can customize these settings according to your project requirements.

---

## Step 4

Click --> Create Table ( Wait until the table status changes to - Active )

---

## Verify Table

Your DynamoDB table should look similar to the following.

| Property | Value |
|----------|-------|
| Table Name | ankittable |
| Partition Key | email |
| Billing Mode | On-demand |
| Table Status | Active |

---

## 🎉 DynamoDB Table Created Successfully

Your application backend can now store user records inside DynamoDB.

---

# 🚀 Phase 4 : Create Public SSL Certificate using AWS Certificate Manager (ACM)

AWS Certificate Manager (ACM) allows you to create and manage free SSL/TLS certificates for your AWS resources.

In this project, ACM will provide HTTPS encryption for our custom domain.

---

## Step 1

Navigate to

```
AWS Console
→ Certificate Manager (ACM)
```

Click -- Request Certificate

```
Choose - Request a Public Certificate
Click  - Next
Enter your domain name - learnwith-ankit.com  [ If you also want SSL support for all subdomains, add another domain. ]
Validation Method - DNS Validation
Leave all remaining settings as default.
Click  -  Request
```


## Step 2

Validate Certificate

After requesting the certificate, scroll down to the **Domains** section.

Click

```
Create records in Route53
```

AWS will automatically create the required DNS validation records.
Wait for Certificate Validation
The certificate status will initially display
Pending Validation
After a few minutes, AWS automatically validates the DNS records.
Once validation is complete, the status changes to Issued

---

# 🚀 Phase 5 : Create Amazon API Gateway (REST API)

Amazon API Gateway allows external users or applications to securely access your AWS Lambda function over HTTPS.

In this project, API Gateway receives HTTP requests from the website and forwards them to the Lambda function for processing.

---

# Step 1 : Create REST API

Navigate to

```
AWS Console
→ API Gateway
→ APIs
→ Create API
```

Select

```
REST API
```

Click

```
Build
```

---

# Step 2 : Configure API

Provide the following details.

| Setting | Value |
|----------|-------|
| API Name | serverless-api-gateway |
| Description | Serverless Web Application API |
| Endpoint Type | Regional |
| IP Address Type | IPv4 |

Click

```
Create API
```

---

# API Created Successfully

Now you'll create API methods that communicate with the Lambda function.

---

# Step 3 : Create GET Method

Inside your API

```
Select                    - Resources
Select the root resource  - /
Click                     - Create Method

```
---

Choose

```
GET
```

---

Configure the integration.

| Setting | Value |
|----------|-------|
| Integration Type | Lambda Function |
| Lambda Proxy Integration | Enable |
| Region | ap-south-1 |
| Lambda Function | serverless-api |

Leave all remaining settings as default.

Click

```
Create Method
```

AWS will automatically add the required permission allowing API Gateway to invoke the Lambda function.

---

# Step 4 : Create POST Method

Again select

```
/
```

Click

```
Create Method
```

Choose

```
POST
```

Configure

| Setting | Value |
|----------|-------|
| Integration Type | Lambda Function |
| Lambda Proxy Integration | Enable |
| Region | ap-south-1 |
| Lambda Function | serverless-api |

Leave all remaining settings as default.

Click

```
Create Method
```

---

## API Structure

Your API should now look similar to

```
/
├── GET
└── POST
```

Both methods invoke the same Lambda function.

---

# Step 5 : Deploy API

Click

```
Deploy API
```

Choose

```
New Stage
```

Stage Name

```
dev
```

Click

```
Deploy
```

---

After deployment, API Gateway generates an Invoke URL similar to

```
https://xxxxxxxxxx.execute-api.ap-south-1.amazonaws.com/dev
```

You can use this URL to test your API before configuring a custom domain.

---

# Verify API

Open the Invoke URL in your browser.

Example

```
https://xxxxxxxxxx.execute-api.ap-south-1.amazonaws.com/dev
```

If everything is configured correctly, your Lambda function should respond successfully.

---

# 🚀 Create Custom Domain Name

Instead of using the default API Gateway Invoke URL, we'll use our own custom domain.

Example

```
https://learnwith-ankit.com
```

or

```
https://api.learnwith-ankit.com
```

---

Navigate to

```
API Gateway
→ Custom Domain Names
→ Add Domain Name
```

---

Provide the following configuration.

| Setting | Value |
|----------|-------|
| Domain Name | learnwith-ankit.com |
| Endpoint Type | Regional |
| IP Address Type | IPv4 |
| ACM Certificate | Select Issued Certificate |
| Security Policy | TLS 1.2 or Latest Available |

Click

```
Add Domain Name
```

Wait until the status becomes Available

---

# Configure API Mapping

Once the custom domain is available,

Scroll down to

```
API Mappings
```

Click

```
Configure API Mapping
```

Configure

| Setting | Value |
|----------|-------|
| API | serverless-api-gateway |
| Stage | dev |
| Path | (none) |

Click --> Save

---

## What does Path = (none) mean?

When the mapping path is

```
(none)
```

your API becomes directly accessible at

```
https://learnwith-ankit.com
```

instead of

```
https://learnwith-ankit.com/dev
```

API Gateway automatically routes requests to the **dev** stage.

---

# Verify API Mapping

Your mapping should look similar to

| API | Stage | Path |
|-----|-------|------|
| serverless-api-gateway | dev | (none) |

---

# Test Custom Domain

Now open your custom domain.

```
https://learnwith-ankit.com
```

or

```
https://api.learnwith-ankit.com
```

If the custom domain does not work, verify the following:

- ACM Certificate Status should be **Issued**
- Custom Domain Status should be **Available**
- API Mapping should point to the correct API and Stage
- Route 53 Alias Record should point to the API Gateway Regional Domain
- API should be deployed after making any changes

---

# 🚀 Phase 6 : Configure Amazon Route 53

Amazon Route 53 is AWS's highly available and scalable DNS (Domain Name System) service.

In this phase, we'll configure Route 53 so that users can access our application using a custom domain instead of the default API Gateway Invoke URL.

---

# Step 1 : Open Hosted Zone

Navigate to

```
AWS Console
→ Route 53
→ Hosted Zones
```

Select your hosted zone

```
learnwith-ankit.com
```

---

# Step 2 : Create DNS Record

Click

```
Create Record
```

Configure the following settings.

| Setting | Value |
|----------|-------|
| Record Name | Leave Blank (for root domain) or `api` (for subdomain) |
| Record Type | A - IPv4 Address |
| Alias | Enable |
| Route Traffic To | Alias to API Gateway API |
| Region | Asia Pacific (Mumbai) - ap-south-1 |
| Endpoint | Select your API Gateway Custom Domain |
| Routing Policy | Simple Routing |

---

# Verify DNS Record

Your Route53 record should look similar to

| Record Name | Type | Alias | Target |
|--------------|------|--------|------------------------------------------|
| api | A | Yes | d-xxxxxxxx.execute-api.ap-south-1.amazonaws.com |

or

| Record Name | Type | Alias | Target |
|--------------|------|--------|------------------------------------------|
| (blank) | A | Yes | d-xxxxxxxx.execute-api.ap-south-1.amazonaws.com |

---

# DNS Propagation

After creating the Route53 record, AWS DNS propagation generally completes within a few minutes.
Sometimes it may take 5–30 Minutes depending on DNS cache.

---

# Verify Website

Open your browser.

If using root domain

```
https://learnwith-ankit.com
```

If using subdomain

```
https://api.learnwith-ankit.com
```

Your application should now be accessible over HTTPS.

---

# Verify Contact Form

Fill in the following fields.

```
Name
Email
Description
```

Click

```
Submit
```

If everything is configured correctly

- Lambda Function will execute
- Data will be inserted into DynamoDB
- Success page will be displayed

---

# Verify DynamoDB

Navigate to

```
AWS Console
→ DynamoDB
→ Tables
→ ankittable
→ Explore Table Items
```

You should see records similar to

| Email | Name | Description |
|--------|------|-------------|
| john@example.com | John | Hello AWS |

Every successful form submission creates a new item inside DynamoDB.

---

# Verify Lambda Logs

Navigate to

```
AWS Console
→ CloudWatch
→ Log Groups
→ /aws/lambda/serverless-api
```

Open the latest Log Stream to verify

- Lambda Execution
- Request Details
- Error Logs (if any)

---

# Complete Project Architecture

```
                    User
                      │
                      │ HTTPS
                      ▼
             learnwith-ankit.com
                      │
                      ▼
               Amazon Route53
                      │
                      ▼
          API Gateway Custom Domain
                      │
                      ▼
             Amazon API Gateway
               (REST API)
             GET /      POST /
                      │
                      ▼
             AWS Lambda Function
                      │
                      ▼
             Amazon DynamoDB Table
                 (ankittable)
                      │
                      ▼
               CloudWatch Logs
```

---

# Project Structure

```
serverless-project/
│
├── lambda_function.py
├── contactus.html
├── success.html
├── requirements.txt
└── README.md
```
---

# Learning Outcomes

After completing this project, you will learn how to

- Create IAM Roles for Lambda
- Deploy Python applications to AWS Lambda
- Create and manage DynamoDB Tables
- Configure REST APIs using API Gateway
- Integrate Lambda with API Gateway
- Enable Lambda Proxy Integration
- Deploy APIs using Stages
- Configure Custom Domain Names
- Create Public SSL Certificates using ACM
- Configure DNS using Route53
- Store application data in DynamoDB
- Monitor Lambda using CloudWatch Logs

---

# Common Issues & Troubleshooting

## 403 Forbidden

Possible Causes

- Incorrect Custom Domain Configuration
- Incorrect Route53 Alias Record
- API not deployed
- Wrong API Mapping

---

## Missing Authentication Token

Possible Causes

- Incorrect API URL
- Wrong HTTP Method
- Invalid Resource Path

Example

❌ Incorrect

```
https://learnwith-ankit.com/dev
```

✅ Correct

```
https://learnwith-ankit.com
```

when API Mapping Path is

```
(none)
```

---

## Lambda Permission Error

Verify the Lambda Resource Policy contains

```
apigateway.amazonaws.com
```

with

```
lambda:InvokeFunction
```

permission.

---

## ACM Certificate Pending Validation

Verify

- DNS Validation Record
- Route53 Hosted Zone
- Domain Ownership

Wait until the certificate status changes to

```
Issued
```
---

# Conclusion

In this project, we successfully built a complete **Serverless Web Application** on AWS without managing any servers.

The application uses:

- Amazon Route53 for DNS management
- AWS Certificate Manager (ACM) for HTTPS
- Amazon API Gateway for REST APIs
- AWS Lambda for backend execution
- Amazon DynamoDB for data storage
- Amazon CloudWatch for monitoring
- AWS IAM for secure access management

This architecture is highly available, scalable, cost-effective, and follows modern serverless best practices.

---

# ⭐ If you found this project helpful, don't forget to Star this repository!
