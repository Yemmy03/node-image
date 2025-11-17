In this workflow, I am automating a complete CI pipeline that takes my source code, builds a Docker image, tags it, pushes it to Amazon ECR, and finally records the image URI for deployment. The workflow runs whenever I push code to the main branch or when I trigger it manually.

I begin by defining a few environment variables.
AWS_REGION tells GitHub Actions which region in AWS I want to work in.
ECR_REPOSITORY is the name of the ECR repository where I want to store my image.
IMAGE_TAG uses the current Git commit SHA so that every image built is uniquely identifiable.
