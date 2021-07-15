# Sysadmin in AWS
- Flexibility is at the heart of the AWS product offering
- At the lowest level, AWS is "simply" an HTTP-based API
- When conflicting permissions are encountered, `Deny` takes precedence over `Allow`

## Denying rights with `NotResource` or `NotAction`
```json
{
    "Statement": [
        { "Action": [
            "s3:*"
        ],
        "Effect": "Allow",
        "NotResource": [
            "arn:aws:s3:::db-backups"
        ]
        }
    ]
}
```

## Policy with conditions
```json
{
    "Statement":[{
            "Effect":"Allow",
            "Action":"s3:*",
            "Resource":"arn:aws:s3:::db-backups",
            "Condition":{
                "Bool":{
                    "aws:SecureTransport":"true"
                }
            }
        }
    ]
}
```
```json
{ 
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "*",
            "Condition": {
                "NotIpAddress": {
                    "aws:SourceIp": ["10.0.0.0/24", "10.10.0.0/24"]
                }
            }
        }
    ] 
}
```

## The AWS security model rely on a shard responsibility model
- User owns the OS's login credentials but AWS bootstraps initial access to that same OS
![](./media/shared_responsibility_model_aws.png)
- AWS is responsible for the physical security of all hardware assets and network infrastructure, software foundation underlying its services. In EC2's case, this includes virtual provisioning infrastructure as well as the issuing of any credentials required to access it
- The customer's security area:
    - AMI, OS, Applications
    - Data in transit, data at rest, data stores
    - Credentials
    - Policies and configurations

## Identify and access management
- manage who and what can access AWS APIs
- Having a well-planed policy based on IAM is an important of AWS security => `defense in depth strategy`
- IAM makes a distinction between authentication (`who is this persion ?`) and authorization (`are they allowed to perform this action ?`)
    - Authentication is handled by users and groups
    - Authorization is handled by IAM policies

### IAM policy
- is a JSON-formatted document describing which actions (who (`user or group`), what (`resource such as EC2, ECS`)) can perform
#### Better solution
- create a set of access credentials that are authorized to perform only the specific actions
- Such as AMI-cleaner, database-backups

## Permission
- is a combination of 2 items: an action and one or more resources
- Actions are namespaced strings that take the form `service_name:Permission`
- Such as `ec2:DeleteSnapshot`

### Create user
```bash
aws iam create-user --user-name ami-cleaner
---
{
    "User": {
        "UserName": "ami-cleaner",
        "Path": "/",
        "CreateDate": "2016-06-01T03:18:35.032Z",
        "UserId": "AIDAILRZI2G4XH3QC6J4W",
        "Arn": "arn:aws:iam::740376006796:user/ami-cleaner"
    }
}
```

### Create access key
```bash
aws iam create-access-key --user-name ami-cleaner
---
{
    "AccessKey": {
        "UserName": "ami-cleaner",
        "Status": "Active",
        "CreateDate": "2016-06-01T03:19:02.919Z",
        "SecretAccessKey": "wSelXh56SYP0f5ZxPkpSNL+kThTqU0nc3JeBNsC2",
        "AccessKeyId": "AKIAJBYS5AQKKUN7MZJQ"
    }
}
```

## Create IAM policy and embeded it to user
```bash
aws iam put-user-policy --user-name ami-cleaner --policy-name ami-cleaner \
--policy-document \
'{"Version":"2008-10-17","Statement":[{"Effect":"Allow", \
"Action":["ec2:DescribeImages","ec2:DeleteSnapshot", \
"ec2:DeregisterImage"],"Resource":["*"]}]}'

aws iamm put-user-policy --user-name ami-cleaner \
--policy-name iam-cleaner \
--policy-document file://./s3-policy.json
```

## Create group and user, then adding user to group
```bash
aws iam create-group --group-name dev_admins --path "/dev/"

aws iam create-user --user-name vana --path "/dev/"

aws iam add-user-to-group --user-name vana --group-name dev_admins
```

## List users and groups
```bash
aws iam list-users --output text

aws iam list-groups --output text
```

## Adding policy to group
`dev_admins.json`
```json
{
    "Statement": {
        "Effect": "Allow",
        "Action": "iam:*",
        "Resource": [
            "arn:aws:iam::740376006796:group/dev/*",
            "arn:aws:iam::740376006796:user/dev/*"
        ]
    }
}
```
```bash
aws iam put-group-policy --group-name dev_admins --policy-name dev_admins --policy-document file://./dev_admins.json
```

## Get credentials report in IAM console
```bash
aws iam get-credential-report | jq -r '.Content' | base64 -d > report.csv
```
### Get access keys
- AWS limit users have 2 access keys
```bash
aws iam list-access-keys --user-name vana
```

### Inactive access key
```bash
aws iam update-access-key --access-key-id vana-accecss-key-id --state Inactive --user-name vana
```

### Update password policy
```bash
aws iam update-account-password-policy --allow-users-to change-password --max-password-age 90 --minimum-password-length 14
```




























