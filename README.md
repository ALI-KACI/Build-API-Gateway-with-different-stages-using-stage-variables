# Build-API-Gateway-with-different-stages-using-stage-variables

## Purpose of the Hands on

 We create an API Gateway with two different stages and integrate it to two different lambda functions.

## Architecture


![API stage drawio](https://github.com/user-attachments/assets/d26b7c38-a86c-47d4-9de1-57c9d81063bd)









### Step-by-Step Implementation

<b> Note:</b> we work on us-east-1 region.


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
      - configure <b>uniquely identify objects</b> (See Number 1 in reference 8)
      - Combine information to ensure uniqueness (See Number 2 in reference 8))
      - Run a code and add ProductionLambda's ARN and API's ARN (See Number 3 in reference 8), to let us see a JSON success output(See Number 4 in reference 8).  (reference: 8-Test ProductionLambda.png)
      - Run a code and add TestingLambda's ARN and API's ARN, to let us see a JSON success output.  (reference: 9-Test TestingLambda.png)


5. Deploy API Gateway with two different stages
    - These Step provide separate environments for testing and production. Implement API Gateway with TestingAPI and ProductionAPI.
      - Deploy API for Testing API stage (reference: 10-TestingAPI.png)
      - Deploy API for Production API stage (reference: 11-ProductionAPI.png)
      - Check the two Stages (reference: 12-Two Stages available.png)
  
       
6. Add stage variables to both stages
    - Here we define the specific Lambda functions associated with each Stage.
      - Add Stage variable in each function.  (reference: 13- example ProductionLambda.png)


7. Test API Gateway
    - Here we ensure that the API Gateway is properly integrated with the Lambda functions, By making GET requests to the endpoints associated with the deployed stages.
      - By copy <b>Invoke URL</b> for the GET requests of <b>ProductionLambda</b>   (reference: 14-Invoke ProductionLambda.png)
      - Similarly for <b>TestingLambda</b>   (reference: 15-Copy URL for GET TestingAPI.png) & (reference: 16-Invoke TestingLambda.png)
   
     


   


> *Lab originally from: [Build API Gateway with different stages using stage variables](https://www.whizlabs.com/labs/build-api-gateway-with-different-stages-using-stage-variables/))*>


> *Architecture Design from: [Design Architecture](https://app.diagrams.net/))*


