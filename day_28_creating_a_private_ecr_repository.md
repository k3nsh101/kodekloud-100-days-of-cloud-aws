### Task

The Nautilus DevOps team has been tasked with setting up a containerized application. They need to create a private Amazon Elastic Container Registry (ECR) repository to store their Docker images. Once the repository is created, they will build a Docker image from a Dockerfile located on the `aws-client` host and push this image to the ECR repository. This process is essential for maintaining and deploying containerized applications in a streamlined manner.

Create a private ECR repository named `nautilus-ecr`. There is a Dockerfile under `/root/pyapp` directory on `aws-client` host, build a docker image using this Dockerfile and push the same to the newly created ECR repo, the image tag must be `latest`.

### Solution

Execute the following lines in the `aws-client` host.

```bash
# Create the repository
aws ecr create-repository --repository-name nautilus-ecr

# Verify the repo is successfully created
aws ecr describe-repositories --repository-names nautilus-ecr

cd /root/pyapp/

# Create the docker image. The name can be anything
docker build -t nautilus-image:latest .

# Tag the image. Ensure the image is tagged as latest
docker tag nautilus-image:latest <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest

# Get aws credentials
showcreds

# Replace <aws_account_id> from above values
aws ecr get-login-password --region "us-east-1" | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com

# Push the image to the ecr
docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest

# Verify the image is contained in the ecr
aws ecr describe-images --repository-name nautilus-ecr
```
