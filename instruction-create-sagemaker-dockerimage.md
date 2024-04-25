### Introduction
This file contain instructions to create docker image for a customize machine learning model, ready to be deployed into SageMaker


### Resource

[Main reference](https://github.com/aws/amazon-sagemaker-examples/blob/main/advanced_functionality/pytorch_extend_container_train_deploy_bertopic/BERTtopic_extending_container.ipynb)

NOTE: when creating a container, we must specify entry point, which is a python script that will be executed when the container is loaded

For reference, consult the [Dockerfile-inference](https://github.com/aws/amazon-sagemaker-examples/blob/main/advanced_functionality/pytorch_extend_container_train_deploy_bertopic/container/Dockerfile-inference)

### NOTES

When trying to get credentials to upload docker image to ECR, note that the default command `get-login` no longer work. The full command is as following:

```
account=$(aws sts get-caller-identity --query Account --output text)
region=$(aws configure get region)
region=${region:-us-west-2} #will us us-west-2 if get region failed
aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${account}.dkr.ecr.${region}.amazonaws.com
```
[source](https://docs.aws.amazon.com/AmazonECR/latest/userguide/registry_auth.html)
