# Build-API-Gateway-with-different-stages-using-stage-variables

## Purpose of the Hands on

Use (AWS) Key Management Service (KMS) for Encrypting data on S3 bucket, EBS volume and AMI & Snaphot. 
KMS provides a central way to manage cryptographic keys for various AWS services and our own applications. AWS KMS makes it easy to create and manage encryption keys, and it offers a range of features and integrations to enhance the security of our data.


## Architecture


![Architecture-KMS](https://github.com/user-attachments/assets/3c74ad6c-bbbf-40d8-86c6-9709a41603a8)






### Step-by-Step Implementation

<b> Note:</b> we work on us-east-1 region. only the bucket that will replicate data from the source bucket in step 5 use ap-south-1 region.
1. Lunch EC2 instance for run CLI commands and perform actions related to the API Gateway and Lambda functions.   (reference: 1-Launch an EC2 Instances.png)
   - Name and Tags: <b>MyEC2CLIInstance</b>
   - instance-type t2.micro
   - WhizKey.pem
   - auto-assign public IPV4 address
   - Security groupe web: <b>MyEC2server_SG</b>
   - inbound rule: SSH from <b>Anywhere</b>
   - Choose existed <b>Role</b> in <b>IAM instance profile</b>

            
2. Create two Lambda functions <ProductionLambda> & <TestingLambda> that will be integrated with the API Gateway.  (reference: 2-Create two Lambda Functions.png)
   - Language use: Python 3.12
   - Use an existing <b>Role</b>for <b>ProductionLambda</b>and another for<b>TestingLambda</b>
   - Deploy the code.(reference: 3-Deploy the code.png)
  
     
3. Create a new <b>API</b> to access Lambda function.
   - Build <b>REST API</b>.
   - NEW API name: Whizlabs API.
   - Create <b>Resource</b> whith name: whizlabsapi (reference: 4-Create API and Resource.png)
   - Create GET Method.
   - Integration Type: Lambda Function.  (reference: 5-Create API & Resource & Method.png)

  
4. Run the CLI command to give Lambda permissions to the API using the EC2 Instance.
   - Run the CLI command through SSH client. (reference: 6-Run the CLI command.png)
   - Configure AWS CLI:
      - Run <b>AWS configure </b> to configure my credentials. (reference: 7-AWS Configure.png)
      - configure <b>uniquely identify objects</b>
      - Combine information to ensure uniqueness
      - 
  
   
  







     
3. Encrypt and Upload an object to S3 bucket.  (reference: 3-Object Image.png)
   - The picture uploaded (Object) and added to Storage Class <b>One Zone-IA</b>
   - Encryption type: SSE-KMS.
  
     
4. Verifying the encryption of the object.  
   - Make it our object public using ACL.
   - <b>Test: By copy Object URL, we are getting error because the file was encrypted using AWS KMS, The access from outside source was restricted. </b> (reference: 4-Copy the Object image.png & 4-Unauthorization to access.png)
   - The object image was opened locally from the source which can decrypt the file by <b>Open</b> button.  (reference: 5-Open Image.png & 5-Open image locally.png)
  
     
5. Cross-Region Replication and Versioning in S3.
   - Create bucket in <b>ap-south-1</b>that will replicate data from the source bucket us-east-1.
   - Uncheck <b>Block all public access</b>
   - Enable Versioning.  (reference: 6-Bucket to replicat data in.png) 
   - On the source bucket we create <b>Replication rule</b> and we choose the second bucket as destination and <b>aws/s3</b> as AWS key.  (reference: 7-Replication Rules.png) 
   - <b>Test: On the source bucket we upload new Object (image test) with Encryption key type: SSE-S3. (reference: 8-Image test successfully replicated to second bucket.png & 8-test object in source bucket.png)</b>


6. Disabling the KMS Key
   - Disable the KMS key.  (reference: 9-Key disabled.png)
   - <b>Test: Access to Image on the bucket source (attached to KMS key not the last one attached to SSE-S3) was denied as the KMS key was disabled.  (reference: 10-Object image.png & 10-Access denied.png)</b>


7. Encryption EBS Volume
   - Create a Volume:
     - name AdminEBS
     - GP2 type
     - 1GiB Size
     - Encryption choose our AWS KMS key (Administrator)  (reference: 11-EBS volume with KMS key encryption.png)

8. Encrypting AMI and Snapshot.
   -  Create an Image with a name <b>AdminUnencrypted</b> on the EC2 instance already created.  (reference:  12-Create an Image.png)
   -  Copy AMI with name <b>AdminEncrypted</b> and check <b>Encypt EBS snapshots of AMI copy</b> and add our KMS key.  (reference:  13-Copy AMI from AdminUnencrypted AMI.png)
   -  search the ID of <b>AdminEncrypted</b> AMI on Snapshot  (reference:  14-AdminEncrypted on snapshot.png)
   -  <b>Test: AMI <b>AdminEncrypted</b> that was copied from <b>AdminUnencrypted</b> has the AWS KMS key.  (reference:  15-AdminEncrypted AMI with AWS KMS key.png)</b>
     





   


> *Lab originally from: [Encrypt S3 bucket, EBS Volume and AMI using AWS KMS](https://www.whizlabs.com/labs/encrypt-s3-bucket-ebs-volume-and-ami-using-aws-kms/))*>


> *Architecture Design from: [Design Architecture](https://app.diagrams.net/))*


