# student-database
deploying student databasee using s3, lambda, api gateway, dynamodb, cloudfront


## Required AWS Services

- **AWS Lambda**: For serverless execution of backend logic.
- **Amazon DynamoDB**: NoSQL database to store and retrieve structured data.
- **Amazon API Gateway**: For creating and exposing RESTful APIs.
- **Amazon S3**: To host static web content.
- **Amazon CloudFront**: To secure and optimize content delivery.

---

## 1. Creating Lambda Functions

- **Service Purpose**: AWS Lambda enables serverless execution of backend code, scaling automatically with usage.
- **Steps**:
  - **Create two Lambda functions** in Python:
    - **Lambda Function 1**: Retrieve data from DynamoDB (GET).
    - **Lambda Function 2**: Insert data into DynamoDB (POST).
  - Assign appropriate **IAM roles** to allow Lambda functions access to DynamoDB.
  - Test the Lambda functions locally or through the AWS Lambda console using test events.

---

## 2. Setting Up DynamoDB

- **Service Purpose**: Amazon DynamoDB is a highly scalable and available NoSQL database for storing application data.
- **Steps**:
  - Create a DynamoDB table with a defined schema (e.g., partition key and optional sort key).
  - Select a capacity mode: **On-Demand** (auto-scaling) or **Provisioned** (manual limits).
  - Use the **Boto3 SDK** within Lambda to perform CRUD operations (e.g., `put_item`, `get_item`).

---

## 3. Configuring API Gateway

- **Service Purpose**: API Gateway exposes your Lambda functions as RESTful API endpoints accessible over HTTP.
- **Steps**:
  - Create a new **API** in API Gateway.
  - Define endpoints and methods (e.g., `/items` with GET and POST methods).
  - Integrate each method with the respective Lambda function.
  - Enable **CORS** for cross-origin frontend communication.
  - Deploy the API to a stage and note the **Invoke URL** for frontend integration.

---

## 4. Setting Up the S3 Bucket

- **Service Purpose**: Amazon S3 provides a scalable and secure storage service to host static website files.
- **Steps**:
  - Create an S3 bucket.
  - Update your frontend script (e.g., JavaScript) with the **API Gateway Invoke URL**.
  - Upload your HTML, CSS, and JavaScript files to the S3 bucket.
  - Enable **Static Website Hosting** in bucket properties.
  - Set the bucket policy to allow public read access for static hosting.

---

## 5. Implementing CloudFront

- **Service Purpose**: Amazon CloudFront is a Content Delivery Network (CDN) that provides secure, low-latency access to web content.
- **Steps**:
  - Create a **CloudFront distribution**, using your S3 bucket as the origin.
  - Configure HTTPS for secure access (using AWS Certificate Manager for a custom domain, if needed).
  - Set up caching to improve performance and reduce latency.
  - Use the **CloudFront URL** as the public-facing endpoint for your application.

---

## Flow Summary

1. **Backend Development**: Lambda → DynamoDB → API Gateway.
2. **Frontend Hosting**: Update scripts → S3 static hosting → CloudFront for secure, optimized delivery.
