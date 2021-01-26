## Output table in command line
```
aws ec2 describe-instances --output table
```

## Using --query option in aws command
```
aws ec2 run-instances --region us-east-1 \
--instance-type t2.micro --image-id ami-43a15f3e \
--output text --query 'Instances[*].InstanceId'
```
- Useful in shell script
```bash
#! /bin/bash
KILL_LIST=$(aws ec2 describe-instances --output text \
--query 'Reservations[*].Instances[*].InstanceId')
aws ec2 terminate-instances --instance-ids $KILL_LIST
```

## Set timeout
```yaml
Globals:
    Function:
    Timeout: 30
    Runtime: python3.8
    Handler: app.lambda_handler
    Environment:
        Variables:
            PARTIES_TABLE: !Ref PartiesTable
            GUESTS_TABLE: !Ref GuestsTable
            REPORTS_BUCKET: !Ref ReportsBucket
```
Using command
```bash
$ sam build --use-container

$ sam package \
    --s3-bucket sam-bucket
    --output-template-file template.yaml

$ sam deploy \
    --template-file template.yaml \
    --stack-name my-stack \
    --capabilities CAPABILITY_IAM
```

## mybucket.yaml
```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: This is a my bucket
Resources:
    MyBucket:
        Type: AWS::S3::Bucket
        Properties:
            AccessControl: PublicRead
```
- Create s3
```
$ aws cloudformation create-stack \
--stack-name mybucket \
--template-body file://MyBucket.yaml
```

## myiamrole.yaml
```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "This is a dummy role"
Resources:
    IamRole:
        Type: AWS::IAM::Role
        Properties:
            AssumeRolePolicyDocument:
                Version: 2012-10-17
                Statement:
                    - Sid: AllowAssumeRole
                      Effect: Allow
                      Principal:
                        AWS:
                          - !Join
                            - ""
                            - - "arn:aws:iam::"
                            - !Ref "AWS::AccountId"
                            - ":user/Admin"
                        Service: "cloudformation.amazonaws.com"
                      Action: "sts:AssumeRole"
            ManagedPolicyArns:
                - "arn:aws:iam::aws:policy/AmazonEC2FullAccess"
                - "arn:aws:iam::aws:policy/AWSCloudformationFullAccess"
Outputs:
    IamRole:
        Value: !GetAtt IamRole.Arn
```
- Create IAM
```
$ aws cloudformation create-stack \
--stack-name iamrole \
--capabilities CAPABILITY_IAM \
--template-body file://IamRole.yaml
```

## Env 
```yaml
Parameters:
    Environment:
        Type: String
        AllowedValued: [ "dev", "test", "prod" ]
        Default: "dev"
Conditions:
    ProdEnv: !Equals [ !Ref Environment, "prod" ]
    TestEnv: !Equals [ !Ref Environment, "test" ]
    DevEnv: !Equals [ !Ref Environment, "dev" ]

Resources:
    ProdDatabase:
        Condition: ProdEnv
        Type: AWS::RDS::DBInstance
        DeletionPolicy: Retain
        Properties:
        # Properties for production database

    TestDatabase:
        Condition: TestEnv
        Type: AWS::RDS::DBInstance
        DeletionPolicy: Snapshot
        Properties:
        # Properties for test database
    DevDatabase:
        Condition: DevEnv
        Type: AWS::RDS::DBInstance
        DeletionPolicy: Delete
        Properties:
        # Properties for dev database
```

## Define task defination in ECS
```yaml
Resources:
    EcsTaskDefinition:
        Type: AWS::ECS::TaskDefinition
        Properties:
        # Some properties...
        ContainerDefinition:
            - Name: mycontainer
            # Some container properties...
            LogConfiguration:
            LogDriver: awslogs
                Options:
                    awslogs-group: myloggroup
                    awslogs-region: !Ref "AWS::Region"
                    awslogs-stream-prefix: ""
```
## Creating a dynamoDB
```yaml
Transform: AWS::Serverless-2016-10-31
Resources:
    DynamoDb:
        Type: AWS::Serverless::SimpleTable
        Properties:
            TableName: myTable
            PrimaryKey:
                Name: id
                Type: String

```
-> Transform -> (SimpleTable in SAM is the same as DynamoDB table in CloudFormation)
```json
{
    "Resources": {
        "DynamoDb": {
            "Type": "AWS::DynamoDB::Table",
            "Properties": {
                "KeySchema": [
                    {
                        "KeyType": "HASH",
                        "AttributeName": "id"
                    }
                ]
            },
            "TableName": "myTable",
            "AttributeDefinitions": [
                {
                    "AttributeName": "id",
                    "AttributeType": "S"
                }
            ],
            "BillingMode": "PAY_PER_REQUEST"
        }
    }
}
```



























