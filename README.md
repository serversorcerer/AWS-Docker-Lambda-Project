# AWS Lambda Python Function Deployment with Docker

A step-by-step guide to developing and deploying an AWS Lambda function using Python 3.9 and Docker.

## **Table of Contents**

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Setup and Deployment](#setup-and-deployment)
  - [Create a Docker Image](#create-a-docker-image)
  - [Tag and Push Docker Image](#tag-and-push-docker-image)
  - [Deploy AWS Lambda Function](#deploy-aws-lambda-function)
- [Example with Real Values](#example-with-real-values)
- [Finding Your Values](#finding-your-values)
- [Problems and Troubleshooting](#problems-and-troubleshooting)
- [Contact](#contact)
- [License](#license)

## **Project Overview**

A streamlined guide to packaging and deploying a Python function on AWS Lambda using Docker, based on a Python 3.9 environment available in a public ECR repository.

## **Prerequisites**

Ensure you have the following setup:

- Docker (Docker Desktop is recommended for M1 Mac users)
- AWS CLI configured with the necessary permissions
- An AWS ECR repository

## **Setup and Deployment**

### **Create a Docker Image**

1. Set up your `Dockerfile` as follows:

    ```dockerfile
    FROM public.ecr.aws/lambda/python:3.9
    COPY app.py ./
    CMD ["app.lambda_handler"]
    ```

2. Build the Docker image:

    ```bash
    docker buildx create --use
    docker buildx build --platform linux/arm64 -t your_image_name:your_image_tag .
    ```

### **Tag and Push Docker Image**

3. Find your Docker image ID using:

    ```bash
    docker images
    ```

4. Tag and push your image to the AWS ECR repository:

    ```bash
    docker tag your_image_id:latest your_account_id.dkr.ecr.your_region.amazonaws.com/your_repository_name:your_image_tag
    docker push your_account_id.dkr.ecr.your_region.amazonaws.com/your_repository_name:your_image_tag
    ```

### **Deploy AWS Lambda Function**

5. Deploy the function to AWS Lambda using:

    ```bash
    aws lambda create-function --function-name your_function_name --role your_role_arn --code image-uri=your_account_id.dkr.ecr.your_region.amazonaws.com/your_repository_name:your_image_tag --package-type Image
    ```

## **Example with Real Values**

A demonstration using non-sensitive details:

    ```bash
    docker buildx build --platform linux/arm64 -t my_sample_project:1.0.0 .
    docker tag abc123:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my_sample_project:1.0.0
    ```

## **Finding Your Values**

Here’s how you find or decide your values:

- **your_image_name:** Create a name that suits your project.
- **your_image_tag:** Assign a version number or tag, e.g., 1.0.0.
- **your_account_id:** Located in the AWS console under “My Account”.
- **your_region:** The AWS region where your resources are, e.g., us-east-1.
- **your_repository_name:** The name of your ECR repository.
- **your_function_name:** A distinctive name for your Lambda function.
- **your_role_arn:** Find this in the IAM section of the AWS console, representing the IAM role your function will utilize.

Remember to replace all placeholder values with your actual details before executing any commands.

## **Problems and Troubleshooting**

Encountering issues? Check these common problems and their solutions:

### **Docker and M1 Mac**

Compatibility issues may arise between AWS Lambda and Docker image architectures.

**Solution:** Adjust the platform flag in the Docker build step.

### **JSON Payload Error**

Errors during AWS Lambda function invocation might be due to the JSON payload.

**Solution:** Ensure to validate and correctly format the JSON payload.

## **Contact**

For further inquiries, contact at [serversorcerer@gmail.com](mailto:serversorcerer@gmail.com).

## **License**

This project is licensed under the MIT license. For more details, refer to the [LICENSE file](./LICENSE) in the repository.
