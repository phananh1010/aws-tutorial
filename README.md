# Introduction
This README contains some notes on how to start using aws. You must already have an AWS account to use this document

# Content
1. Setup aws
[Referece](https://www.freecodecamp.org/news/aws-cli-tutorial-install-configure-understand-resource-environment/)
Step-by-step
a. Download aws package, and install on Linux environment
b. Configure access to the AWS with this command
```
aws configure
```
Note that you are not recommended to use root credential for access key, the alternative is to create IAM role and the associated accesskey.
*Let's create a user with Dashboard access and belong to Admin group.
*Then sign out of your root account and log in to your admin IAM account
*Use the following document to create access key for your admin account.
[guide](https://www.msp360.com/resources/blog/how-to-find-your-aws-access-key-id-and-secret-access-key/#:~:text=1%20Go%20to%20Amazon%20Web,and%20Secret%20Access%20Key)%20option.)

3. Create a Virtual Private Network to host the SageMaker instance following the following [guide](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-example-dev-test.html) 

4. Create SageMaker studio and follow instructions in these guides:
a. This [guide](https://aws.amazon.com/tutorials/machine-learning-tutorial-deploy-model-to-real-time-inference-endpoint/) does not work as Python 3.7 is longer accepted, but it is a good reference point
b. This is the working [guide](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html#onboard-quick-start-defaults). Use the setup for single user on SageMaker landing page
5. 
