# Download S3 Buckets to local machine recursively
The following demonstates a way to use the AWS CLI (Command Line Tools) to download Amazon S3 buckets into your local machine.

### 1: Install the AWS CLI tool
Download from [here](https://aws.amazon.com/cli/).

### 2: Get AWS access key ID and secret
Open the following link and login to your AWS account: https://console.aws.amazon.com/iam/home?#/security_credentials    
Select "Access keys" from the list, click on "Create New Access Key". Note down the ID and secret.

### 3. Configure AWS CLI to use the key ID and secret
Open a command line tool e.g. Git Bash and run the following:
```
aws configure
```

You will be prompted to enter your key ID and secret:
```
AWS Access Key ID: 
AWS Secret Access Key:
```
Copy and paste your ID and secret. Leave the remaining 2 options Default Region and Default Output blank, and hit "enter" until it completes.


### 4. Get the list of buckets in your AWS account
In your command line tool, run:
```
aws s3 ls | awk '{print $NF}' > bucket-list.txt
```
This will get the names of all the buckets currently in your AWS account and puts them all in a file called bucket-list.txt

### 5. Strip \r characters from bucket names
In your command line tool, run:
```
tr -d '\r' < bucket-list.txt > bucket-list-clean.txt
```
For more information on why there are '\r' characters, see this [StackOverflow article](https://superuser.com/questions/489180/remove-r-from-echoing-out-in-bash-script).

### 6. Start downloading all S3 buckets
In your command line tool, run:
```
while read in; do aws s3 sync s3://$in ./$in; done < bucket-list-clean.txt
```
This command reads bucket-list-clean.txt line by line, and runs "aws s3 sync" on each of the bucket names to download the files into local folders of the same name.

## References
https://stackoverflow.com/questions/8659382/downloading-an-entire-s3-bucket

## License
This script is unlicensed. Feel free to use it as you please.