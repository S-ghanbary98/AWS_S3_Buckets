# AWS EC2 Instance With S3 Bucket

- In this example an EC2 instance will be and will be connected to a s3 bucket.
- Various different aws command will be carried out to create, read, update, and delete files in the s3 bucket.
![alt text](ec2.png)
## Running EC2 Instance And Connecting to S3 Bucket
- Firstly we set up an ec2 instance with the following setting and dependencies.
##### AMI
`Ubuntu Server 16.04 LTS (HVM), SSD Volume Type`
##### Instance Type
`t2.micro`
##### Configure Instance 
```
Subnet:   subnet ............default1A
Auto-assign Public IP: Enable
```
##### Add Tags
``` 
Name: eng89_shervin
Description: none 
```
##### Configure Security Group 
- First Inbound protocol should be for the `SSH` at default port 22 and the localhost `IPV4`.
- ` HTTP` protocol also for access from local computer.

#### Dependencies
- SSH into EC2 instance and download the following dependencies.
```python
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa

sudo apt update
sudo apt install python3.8
sudo apt install python-pip
pip install boto3
sudo apt install awscli
```

Once all dependencies have been set up type `aws configure` where you will shortly be prompted for
- Access Key
- Secret Access Key
- Region
- Location

Once all the required information has been typed in you can successfully create a bucket and files as well as perform CRUD.

### Creating an S3 Bucket
- `aws s3 mb s3://bucket_name` where mb stands for 'make bucket'.
### Uploading Files to bucket
- `aws s3 cp test.txt s3://bucket_name` where cp stands for 'copy'.
### Download file from bucket
- `sudo aws s3 cp s3://bucket_name/test.txt new_name`.

# Boto3 

- Locate folder were `credentials` and `config` files are on the localhost. Create a .py file that will contain the code.

### Printing all S3 buckets
```python
import boto3
s3 = boto3.client('s3')
response = s3.list_buckets()

print('Existing buckets:')
for bucket in response['Buckets']:
    print(f'  {bucket["Name"]}')
```

### Creating a S3 Bucket
```python
s3_client = boto3.client('s3')
s3_client.create_bucket(Bucket=bucket_name)
```

### Downloading file from S3 Bucket
```python
s3_client = boto3.client('s3')
s3_client.download_file(bucket_name, object_name_in_bucket, file_name)
```

### Uploading a file to a S3 Bucket 
```python
s3_client = boto3.client('s3')
s3_client.upload_file(file_name, bucket, object_name)
```

### Deleting Object in S3 bucket

```python
s3_client = boto3.client('s3')
s3_client.delete_object(Bucket=bucket_name, object_name)
```

### Deleting S3 Bucket 
```python
s3_client = boto3.client('s3')
s3_client.delete_bucket(Bucket=bucket_name)
```