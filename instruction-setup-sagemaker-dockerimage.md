# This is a some quick notes while setting up docker images for sagemaker

## Deploy source code:
If you have source code from a Github repo, you could write code in jupyter to download the code to sagemaker instance, then copy it to sagemaker dockerimage
First, provide user name, password, authentication methods, and tokens
```
!echo "https://phananh1010:[TOKEN]@github.com" >> ~/.git-credentials
!rm ~/.gitconfig
!git config --global user.name "Anh Nguyen"
!git config --global user.email phananh1010@yahoo.com
!git config --global github.user phananh1010@yahoo.com
!git config --global credential.helper store
```

Then copy the code to the container directory
```
!cp [GITHUB_DIR]/*.py ./container/[CODE_DIR]
```

Download any required files to the project folders
```
import boto3
from botocore.exceptions import NoCredentialsError

def download_file_from_s3(bucket_name, s3_object_key, local_file_path):
    """
    Download a file from an S3 bucket.
    
    :param bucket_name: str, the name of the S3 bucket
    :param s3_object_key: str, the key of the object in the S3 bucket
    :param local_file_path: str, the local path where the file will be saved
    """
    # Initialize a session using your credentials
    s3 = boto3.client('s3')
    
    try:
        # Download the file
        s3.download_file(bucket_name, s3_object_key, local_file_path)
        print("Download successful.")
    except NoCredentialsError:
        print("Credentials not available.")
    except Exception as e:
        print(f"An error occurred: {e}")

bucket_name = 'sagemaker-deployment'

object_key_list = ['test.pickle', 'train.pickle', 'prompt_embeddings.pt', 'word_embeddings.pt']#'checkpoint_batch_epoch1.pth', 
local_path_list = [ './container/[CODE_DIR]/data/', './container/[CODE_DIR]/data/', './container/[CODE_DIR]', './container/[CODE_DIR]/']#

# Example usage
for idx, object_key in enumerate(object_key_list):
    local_path = local_path_list[idx] + object_key
    print (f'downloading from bucket: {bucket_name}, key: {object_key}, to: {local_path}')
    download_file_from_s3(bucket_name, object_key, local_path)
```
