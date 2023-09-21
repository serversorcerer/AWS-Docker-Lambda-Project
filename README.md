# Simple AWS Lambda Python Deployment Using Docker

This guide helps you deploy a Python function to AWS Lambda using Docker, leveraging a Python 3.9 environment. Follow the steps below to get your function up and running.

## **Table of Contents**
- [Project Overview](#project-overview)
- [Requirements](#requirements)
- [Setup and Deployment](#setup-and-deployment)
- [Contact](#contact)
- [License](#license)

### **Project Overview**

We are setting up a Python function to run on AWS Lambda through a Docker container, using a public ECR Python 3.9 image. 

### **Requirements**
- Docker software installed (M1 Mac chip users are covered)
- AWS CLI configured with the necessary permissions
- An established AWS ECR repository

### **Setup and Deployment**

**Step 1:** **Docker Image Preparation**
   
Create a Dockerfile using the structure below, substituting `app.py` with your Python file and `app.lambda_handler` with your function name:

```dockerfile
FROM public.ecr.aws/lambda/python:3.9
COPY app.py ./
CMD ["app.lambda_handler"]

Build your Docker image using the commands below, changing my_python_app and 1.0 to your image name and tag:

docker buildx create --use
docker buildx build --platform linux/arm64 -t my_python_app:1.0 .

Step 2: Tagging and Pushing the Docker Image

Find your Docker image ID with:

docker images

Then, tag and push your image to the AWS ECR using the following commands. Make sure to replace the placeholders with your details:

docker tag your_image_id:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my_ecr_repo:1.0
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my_ecr_repo:1.0

Step 3: AWS Lambda Function Deployment

Deploy your function using the command below, replacing placeholders with your specific details:

aws lambda create-function --function-name my_lambda_function --role arn:aws:iam::123456789012:role/execution_role --code image-uri=123456789012.dkr.ecr.us-east-1.amazonaws.com/my_ecr_repo:1.0 --package-type Image

Contact

For queries or support, email us at youremail@example.com.

License

This project is MIT licensed. Refer to the LICENSE file for more details.

### **Instructions to Replace Placeholders**

- `app.py` and `app.lambda_handler`: Your Python file and function name.
- `my_python_app` and `1.0`: Choose an image name and tag.
- `123456789012`, `us-east-1`, `my_ecr_repo`: Your AWS account ID, region, and ECR repository name (found in the AWS Management Console).
- `your_image_id`: Found using the command `docker images`.
- `my_lambda_function` and `arn:aws:iam::123456789012:role/execution_role`: Your chosen Lambda function name and AWS IAM role ARN (found in the AWS IAM console).

🔐 **Safety Note**: Keep your AWS account ID and IAM role ARN confidential to maintain security.
