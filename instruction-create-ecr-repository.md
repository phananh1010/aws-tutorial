Here is the instruction allowing you to create and upload a private repository to AWS ECR. This is a typical required step to train/deploy SageMaker model.


## Step 1: create ECR repository


## Step 2: add permission to the associated IAM account. Here is the JSON except for all permission needed to upload/overwrite exsisting ECR repo. Note that you need to create an empty repo before hand as these permission is insufficient to create new repo.
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecr:InitiateLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:CompleteLayerUpload"
      ],
      "Resource": "arn:aws:ecr:us-east-1:[your-iam-id]:repository/bert-topic-training-example"
    }
  ]
}
```

## Step 3: download existing docker image and upload. Here is an example bash script to do this task
```
#!/usr/bin/env bash

# This script shows how to build the Docker image and push it to ECR to be ready for use
# by SageMaker.

# The argument to this script is the image name. This will be used as the image on the local
# machine and combined with the account and region to form the repository name for ECR.
image=$1

dockerfile=${2:-Dockerfile}

if [ "$image" == "" ]
then
    echo "Usage: $0 <image-name>"
    exit 1
fi

account=$(aws sts get-caller-identity --query Account --output text)
# Get the region defined in the current configuration (default to us-west-2 if none defined)
region=$(aws configure get region)
region=${region:-us-west-2}

fullname="${account}.dkr.ecr.${region}.amazonaws.com/${image}:latest"
echo "ECR image fullname: ${fullname}"
# If the repository doesn't exist in ECR, create it.

aws ecr describe-repositories --repository-names "${image}" > /dev/null 2>&1

if [ $? -ne 0 ] ; then aws ecr create-repository --repository-name "${image}" > /dev/null ; fi;

# Get the login command from ECR and execute it directly
export _DOCKER_REPO="$(aws ecr get-authorization-token --output text --query 'authorizationData[].proxyEndpoint')"
aws ecr get-login-password --region ${region} | sudo docker login --username AWS --password-stdin ${account}.dkr.ecr.${region}.amazonaws.com

# Get the login command from ECR in order to pull down the SageMaker PyTorch image
#NOTE: for local non-sagemaker, must use `sudo login`
aws ecr get-login-password --region ${region} | sudo docker login --username AWS --password-stdin "763104351884.dkr.ecr.${region}.amazonaws.com"

# Build the docker image locally with the image name and then push it to ECR
# with the full name.

sudo docker build -f ${dockerfile} -t ${image} . --build-arg REGION=${region}
sudo docker tag ${image} ${fullname}
sudo docker push ${fullname}

# an alternative and simplified command that can be used instead of the code above is
# sm-docker build ${1}
```
