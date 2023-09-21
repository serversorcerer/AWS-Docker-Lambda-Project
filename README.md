# **Simple AWS Lambda Python Deployment Using Docker**

This guide will aid you in deploying a Python function to AWS Lambda using Docker, all under a Python 3.9 environment. Just follow the easy steps outlined below to have your function running in no time.

## **📝 Table of Contents**
- [Project Overview](#project-overview)
- [Requirements](#requirements)
- [Setup and Deployment](#setup-and-deployment)
- [Finding Your Details](#finding-your-details)
- [Contact](#contact)
- [License](#license)

### **🌟 Project Overview**

Setting up a Python function to run on AWS Lambda through a Docker container couldn't be simpler. This setup utilizes a Python 3.9 image from a public ECR repository.

### **🛠 Requirements**
- Docker software (M1 Mac chip users are covered)
- AWS CLI with necessary permissions
- A ready AWS ECR repository

### **🚀 Setup and Deployment**

#### **Step 1: Docker Image Preparation**
   
Prepare your Dockerfile as shown below:

```dockerfile
FROM public.ecr.aws/lambda/python:3.9
COPY app.py ./
CMD ["app.lambda_handler"]

Next, build your Docker image with:

docker buildx create --use
docker buildx build --platform linux/arm64 -t my_python_app:1.0 .

Step 2: Tagging and Pushing the Docker Image

Identify your Docker image ID using:

docker images

Tag and push your image to the AWS ECR:

docker tag your_image_id:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my_ecr_repo:1.0
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my_ecr_repo:1.0

Step 3: AWS Lambda Function Deployment

Deploy your function using AWS CLI:

aws lambda create-function --function-name my_lambda_function --role arn:aws:iam::123456789012:role/execution_role --code image-uri=123456789012.dkr.ecr.us-east-1.amazonaws.com/my_ecr_repo:1.0 --package-type Image

🔎 Finding Your Details

To replace the placeholders in the commands, here is how you find the real values:

	•	app.py and app.lambda_handler: Use your Python filename and the function within it that you wish to run.
	•	my_python_app and 1.0: Decide on a name and a tag for your Docker image.
	•	123456789012, us-east-1, my_ecr_repo: Find these in your AWS console; they represent your AWS account ID, region, and the name of your ECR repository.
	•	your_image_id: Use the docker images command to find this ID.
	•	my_lambda_function and arn:aws:iam::123456789012:role/execution_role: Name your AWS Lambda function and find your role ARN in the AWS IAM console.

🔒 Safety Note: Always keep your AWS IAM role ARN and AWS account ID confidential to maintain your project’s security.

📬 Contact

For any queries or further assistance, feel free to reach out at serversorcerer@gmail.com.

📜 License

This project is licensed under the terms of the MIT license. Check the LICENSE file in the repository for more details.
