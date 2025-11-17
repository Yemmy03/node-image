# CI Pipeline for Building and Pushing Docker Images to Amazon ECR

This workflow automates a complete CI pipeline that takes my source code, builds a Docker image, tags it, pushes it to Amazon ECR, and records the final image URI for deployment. The workflow runs whenever I push code to the main branch or when I trigger it manually through the GitHub Actions interface.

## Environment Variables

I begin by defining a few environment variables that the pipeline relies on.

- **AWS_REGION** specifies the AWS region where I want to operate.
- **ECR_REPOSITORY** is the name of the ECR repository where my image will be stored.
- **IMAGE_TAG** uses the current Git commit SHA so that every image is uniquely identifiable.

## Workflow Steps

### 1. Check out the repository  
The job starts by checking out my source code so GitHub Actions has access to the project files.

### 2. Configure AWS credentials  
I load my AWS access key and secret key from GitHub Secrets. This authenticates all AWS CLI commands and sets the region for the workflow.

### 3. Log in to Amazon ECR  
I authenticate Docker to Amazon ECR using the official login action. This gives me a temporary authentication token that allows Docker to push images securely.

### 4. Ensure the ECR repository exists  
Before pushing any image, I check if the target ECR repository exists. If it does not exist, I create it. This prevents the workflow from failing because of missing infrastructure.

### 5. Build the Docker image  
I use the docker build command to build the image from my Dockerfile. The image is tagged with the repository name and the commit SHA to ensure it is distinct.

### 6. Tag the image for ECR  
I construct the full image URI which includes my AWS registry address, the repository name, and the image tag. I tag the local image with this full URI and store it in the GitHub environment for later use.

### 7. Push the image to ECR  
I push the tagged image to ECR using the docker push command. This uploads the container image and makes it available for deployment.

### 8. Save the image URI  
I write the final image URI to a file named image.txt. I then upload this file as a workflow artifact so that I or other workflows can easily retrieve the exact image that was produced.

## Summary

This workflow provides a fully automated container pipeline. I authenticate with AWS, prepare the repository, build the Docker image, tag it, push it to ECR, and save the final image URI as an artifact. This gives me consistent and repeatable deployments suitable for production environments.
