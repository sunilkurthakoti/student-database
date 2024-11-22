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

      ![Screenshot (61)](https://github.com/user-attachments/assets/ee8fa2e1-7301-4694-8374-38ec5604b2ee)  
      ![Screenshot (62)](https://github.com/user-attachments/assets/2e165aef-0a58-45bc-bd8b-77672fe1798d)  
      ![Screenshot (63)](https://github.com/user-attachments/assets/4e02037b-7051-4c1f-9170-d36866e65b04)  

    - **Lambda Function 2**: Insert data into DynamoDB (POST).  

      ![Screenshot (64)](https://github.com/user-attachments/assets/1ee470ea-177d-4c5b-b0f1-3852db8164c4)  
      ![Screenshot (65)](https://github.com/user-attachments/assets/62524c15-54de-4401-8f55-dec11bb23f9c)  
      ![Screenshot (66)](https://github.com/user-attachments/assets/79df604d-349d-4aa9-b6f7-41970f346db6)  

  - Assign appropriate **IAM roles** to allow Lambda functions access to DynamoDB.  

    ![Screenshot (59)](https://github.com/user-attachments/assets/e2bd75e9-39fa-45db-8073-5ea31ceabf91)  

  - Test the Lambda functions locally or through the AWS Lambda console using test events.  

    ![Screenshot (68)](https://github.com/user-attachments/assets/6cbef9c0-8d20-411a-b690-6a88cc6591d6)  

---

## 2. Setting Up DynamoDB  

- **Service Purpose**: Amazon DynamoDB is a highly scalable and available NoSQL database for storing application data.  
- **Steps**:  
  - Create a DynamoDB table with a defined schema (e.g., partition key and optional sort key).  

    ![Screenshot (60)](https://github.com/user-attachments/assets/38ac05df-f2d0-4810-9bd0-87344ddfc3d3)  

  - Select a capacity mode: **On-Demand** (auto-scaling) or **Provisioned** (manual limits).  
  - Use the **Boto3 SDK** within Lambda to perform CRUD operations (e.g., `put_item`, `get_item`).  

    ![Screenshot (69)](https://github.com/user-attachments/assets/0e26fc4c-f76a-4782-b385-1ed990b04815)  

---

## 3. Configuring API Gateway  

- **Service Purpose**: API Gateway exposes your Lambda functions as RESTful API endpoints accessible over HTTP.  
- **Steps**:  
  - Create a new **API** in API Gateway.  

    ![Screenshot (70)](https://github.com/user-attachments/assets/fe0c46ab-668c-4220-8093-ee875177ff5b)  

  - Define endpoints and methods (e.g., `/items` with GET and POST methods).  

    ![Screenshot (71)](https://github.com/user-attachments/assets/9674000b-3f8f-436a-a956-2f2265fc3f74)  

  - Integrate each method with the respective Lambda function.  

    ![Screenshot (73)](https://github.com/user-attachments/assets/6b78018b-19d8-4c58-a442-95470a24b3ff)  

  - Enable **CORS** for cross-origin frontend communication.  
  - Deploy the API to a stage and note the **Invoke URL** for frontend integration.  

    ![Screenshot (72)](https://github.com/user-attachments/assets/bd2c6766-24cb-4393-9cf3-2151f37caa77)  

---

## 4. Setting Up the S3 Bucket  

- **Service Purpose**: Amazon S3 provides a scalable and secure storage service to host static website files.  
- **Steps**:  
  - Create an S3 bucket.  
  - Update your frontend script (e.g., JavaScript) with the **API Gateway Invoke URL**.  
  - Upload your HTML, CSS, and JavaScript files to the S3 bucket.  

    ![Screenshot (74)](https://github.com/user-attachments/assets/174be261-dfde-46b8-91cb-1849bcc602a1)  

  - Enable **Static Website Hosting** in bucket properties.  
  - Set the bucket policy to allow public read access for static hosting.  

    ![Screenshot (75)](https://github.com/user-attachments/assets/2493c281-c948-4b60-bb4a-3c73496ab24c)  
    ![Screenshot (77)](https://github.com/user-attachments/assets/2bb66958-7ba5-4599-bbdf-d6798f388937)  

---

## 5. Implementing CloudFront  

- **Service Purpose**: Amazon CloudFront is a Content Delivery Network (CDN) that provides secure, low-latency access to web content.  
- **Steps**:  
  - Create a **CloudFront distribution**, using your S3 bucket as the origin.  

    ![Screenshot (79)](https://github.com/user-attachments/assets/4d043941-e101-46eb-9db4-72a939658d2e)  

  - Configure HTTPS for secure access (using AWS Certificate Manager for a custom domain, if needed).  
  - Set up caching to improve performance and reduce latency.  
  - Use the **CloudFront URL** as the public-facing endpoint for your application.  

    ![Screenshot (81)](https://github.com/user-attachments/assets/10f49e75-ca34-4cf9-9edf-5964957c6530)  
    ![Screenshot (78)](https://github.com/user-attachments/assets/231cafea-6958-41bc-9394-02e924db9b33)  

---

## Flow Summary  

1. **Backend Development**: Lambda → DynamoDB → API Gateway.  
2. **Frontend Hosting**: Update scripts → S3 static hosting → CloudFront for secure, optimized delivery.  
