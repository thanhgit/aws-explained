# Serverless Application Model (SAM)

## Note:
- Must have a bucket when you want to deploy a SAM application via a CLI
- You can't use the SAM CLI to delete a SAM application, Instead of, using: `aws cloudformation delete-stack --stack-name my-stack --region my-region --profile my-profile`
- Build app and run local tests
    - Build command: `sam build --use-container`
    - It will pull a Docker image and bubild the source function
    - Test command: `sam local invoke my_function --event event.json`
    - Start API: `sam local start-api` -> call API: `curl localhost:3000/hello`
- Packaging application
    - Create S3 command: `aws s3 mb s3://sam-bucket`
    - Push sam: `sam package --s3-bucket sam-bucket --output-template-file template.yaml`
- Deploying application:
    - Command: `sam deploy --template-file template.yaml --stack-name my-stack --capabilities CAPABILITY_IAM`
- Watching logs 
    - Command: `sam logs -n my-function --stack-name my-stack --tail`

## What is serverless?
- Serverless is a way of developing, running, and operation applications without to manage underlying infrastructure
- SAM can perform all of its operation out of the box. Such as:
    - Building, testing locally
    - Provisioning serverless stacks to AWS
- SAM template is designed to be used with serverless applications and services
- SAM template supports:
    - AWS::Serverless::API : creates an API gateway and related resources
    - AWS::Serverless::Application : creates a serverless application
    - AWS::Serverless::Function : Creates a lambda function and related resources
    - AWS::Serverless::HttpApi : Creates an HTTP API gateway and related resources
    - AWS::Serverless::LayerVersion : Creates a lambda layer
    - AWS::Serverless::SimpleTable : creates a simplified DynamoDB table (only a primary key without a sort key)
