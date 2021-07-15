# Amazon web services

## [VPC](./vpc.md)
## [IAM](./sysadmin/iam.md)

## [Overview Security AWS](./sysadmin/security_aws.md)
## [EC2](./ec2.md) (Elastic compute cloud)
- [Lifecycle of EC2](https://jayendrapatil.com/tag/disableapitermination/)

## [ECS](./ecs.md)

## [SAM](./cloudformation/sam.md) and [Cloudformation](./cloudformation/cloudformation.md)

## [DynamoDB](./dynamodb/dynamodb.md)

## [SNS](./sns.md)

## [SQS](./sqs.md)
### References
https://github.com/PacktPublishing/Mastering-AWS-CloudFormation

### There are 2 types of policy:
- Identity based policy: are attached to an IAM identity (user/ group/ role)
- Resource based policy: are attached to a resource 
- For example, 
    - Allowing DescribeTable, Query and Scan to all resource
    ```json
    {
        "Version": "2012-10-17",
        "statement": [
            {
                "Sid": "ListTables",
                "Effect": "Allow",
                "Action": [
                    "dynamodb:ListTables"
                ],
                "Resource": "*"
            }
        ]
    }
    ```
    - Specifically
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
            "Sid": "DescribeQueryScanEmployeeTable",
            "Effect": "Allow",
            "Action": [
                "dynamodb:DescribeTable",
                "dynamodb:Query",
                "dynamodb:Scan"
            ],
            "Resource": "arn:aws:dynamodb:us-east-1:account-id:table/employee"
            }
        ]
    }
    ```

## Terraform
- [Terraform security](https://speakerdeck.com/garethr/shifting-terraform-security-left)
- [Terraform common](./terraform/terraform_common.md)
- [Terraform in detail](./terraform/terraform_in_detail.md)
- [Terraform structure](./terraform/terraform_structure.md)
- [Terraform pattern](./terraform/terraform_pattern.md)
- [Terraform example](./terraform/terraform_example.md)

## [Penetration testing](./sysadmin/security_penetration.md)
## Best practise
- [Best Practices of Infrastructure-as-Code with Terraform](https://speakerdeck.com/joatmon08/best-practices-of-infrastructure-as-code-with-terraform)

## Tools
- [Create flow chart with markdown](https://github.com/mermaid-js/mermaid)
- [Create aws architecture with cloudcraft](https://www.cloudcraft.co/)
- [pre-commit for validating configuration](https://github.com/antonbabenko/pre-commit-terraform)
- [NMAP](./sysadmin/nmap)
- [Online tools](./tools.md)