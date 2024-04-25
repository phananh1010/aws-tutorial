### Introduction
This file contain instructions to create docker image for a customize machine learning model, ready to be deployed into SageMaker


### Resource

[Main reference](https://github.com/aws/amazon-sagemaker-examples/blob/main/advanced_functionality/pytorch_extend_container_train_deploy_bertopic/BERTtopic_extending_container.ipynb)

NOTE: when creating a container, we must specify entry point, which is a python script that will be executed when the container is loaded

For reference, consult the [Dockerfile-inference](https://github.com/aws/amazon-sagemaker-examples/blob/main/advanced_functionality/pytorch_extend_container_train_deploy_bertopic/container/Dockerfile-inference)


