# AWS Lambda Python Function Deployment with Docker

A guide to developing and deploying a Python function to AWS Lambda using a Docker container.

## Table of Contents

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Setup and Deployment](#setup-and-deployment)
  - [Create a Docker Image](#create-a-docker-image)
  - [Tag and Push Docker Image](#tag-and-push-docker-image)
  - [Deploy AWS Lambda Function](#deploy-aws-lambda-function)
- [Troubleshooting](#troubleshooting)
- [Contact](#contact)
- [License](#license)

## Project Overview

This repository provides a guide on how to package and deploy a Python function to AWS Lambda using a Docker container.

## Prerequisites

Ensure that you have the following set up before starting:

- Docker installed on your system
- AWS CLI configured with the necessary permissions
- An AWS ECR repository created

## Setup and Deployment

Here are the step-by-step instructions to set up and deploy your function:

### Create a Docker Image

1. Prepare your `Dockerfile` using the script below:

    ```dockerfile
    FROM public.ecr.aws/lambda/python:3.9
    COPY app.py ./
    CMD ["app.lambda_handler"]
    ```

2. Open your terminal and build the Docker image using the following command (replace `your_image_name` and `your_image_tag` with your details):

    ```bash
    docker build -t your_image_name:your_image_tag .
    ```

### Tag and Push Docker Image

3. Tag and push your Docker image to your AWS ECR repository using the commands below (replace placeholders with your details):

    ```bash
    docker tag your_image_name:your_image_tag your_account_id.dkr.ecr.your_region.amazonaws.com/your_repository_name:your_image_tag
    docker push your_account_id.dkr.ecr.your_region.amazonaws.com/your_repository_name:your_image_tag
    ```

### Deploy AWS Lambda Function

4. Deploy your function to AWS Lambda using the following command (replace placeholders with your details):

    ```bash
    aws lambda create-function --function-name your_function_name --role your_role_arn --code image-uri=your_account_id.dkr.ecr.your_region.amazonaws.com/your_repository_name:your_image_tag --package-type Image
    ```

## Troubleshooting

In case you run into issues, here are some possible solutions:

- **JSON Payload Error:** Ensure your JSON payload is correctly formatted. Use online tools to validate the format.
- **Docker Image Compatibility:** If using an M1 Mac, ensure that the Docker image is compatible with AWS Lambda by setting the correct architecture during the Docker build process.

## Contact

If you have questions or run into issues, feel free to reach out at [serversorcerer@gmail.com](mailto:serversorcerer@gmail.com).

## License

This project is licensed under the MIT license. For more details, refer to the LICENSE file in this repository.
