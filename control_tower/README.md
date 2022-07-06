# Control tower
- ### It provides the easiest way to set up and govern a secure, multi-account AWS environment based on best practices of AWS

## AWS Landing Zone solution
- ### The setup of AWS Organizations and a set of AWS accounts tasked with different parts of your overall cloud strategy
- ### A centralized logging account is set up that compiles CloudTrail logs for efficient analysis
- ### A security account is set up to monitor your cloud resources across accounts using Amazon GuardDuty
- ### A shared services account is provisioned for any services like Active Directory that need to be accessible across workloads
- ### An Account Vending Machine is created as a Service Catalog product, enabling permitted users to create AWS accounts in an efficient and secure way


## Setting up an automated landing zone with AWS Control Tower
### Step 1: `Create a new account to serve as the Control Tower master`
### Step 2: Spawn two additional core accounts: `Log Archive` and `Audit`
### Step 3: Usubg the `AWS Service Catalog` to create a new provisioned account

## Guardrail examples
- ### `IAM security`: require MFA for root user
- ### `Data security`: disallow public read access to S3 bucket
- ### `Network security`: disallow internet connection via RDP
- ### `Audit logs`: enable AWS Cloudtrail and Config
- ### `Monitoring`: enable AWS Cloudtrail integration with CloudWatch
- ### `Encryption`: ensure encryption of AWS EBS volumes attached to EC2 instances
- ### `Drift`: disallow changes to AWS config rules set up by AWS Control Tower