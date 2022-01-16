# IAM Role AWS

## Create S3 and allowing to access to it
```bash
aws s3 mb s3://images

aws s3 cp avatar.png s3://images/
```

### Create IAM role
file://assume-role.json
```json
{
    "Role": {
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": {
                "Action": "sts:AssumeRole",
                "Effect": "Allow",
                "Principal": {
                    "Service": "ec2.amazonaws.com"
                }
            }
        },
        "RoleId": "AROAIJSFDM5WJNR2M5ZVI",
        "CreateDate": "2016-06-07T05:22:34.145Z",
        "RoleName": "avatar-image",
        "Path": "/",
        "Arn": "arn:aws:iam::740376006796:role/images"
    }
}
```
```bash
aws iam create-role --role-name avatar-image --assume-role-policy-document file://./assume-role.json
```
With trust policy document
```json
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Principal": {"Service": "ec2.amazonaws.com"},
        "Action": "sts:AssumeRole"
    }
}
```

### Create IAM policy
file://avatar-image-policy.json
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1465279863000",
            "Effect": "Allow",
            "Action": [
                "s3:DeleteObject",
                "s3:GetObject",
                "s3:ListBucket",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::images/*",
                "arn:aws:s3:::images"
            ]
        }
    ]
}
```
```bash
aws iam put-role-policy --role-name avatar-image --policy-name avatar-image-policy --policy-document file://./avatar-image-policy.json
```

### Create instance profile, which will let us launch an EC2 instance using the role
```bash
aws iam create-instance-profile --instance-profile-name avatar-image --output text
---
INSTANCEPROFILE arn:aws:iam::740376006796:instance-profile/image-resizing
2016-06-07T06:33:09.622Z
AIPAIAUTOO3GIHBKVG67G
images /
```
### Adding role to instance profile
```bash
aws iam add-role-to-instance-profile --instance-profile-name avatar-image --role-name avatar-image
```

### Create EC2 instance attach to iam-instance-profile
```bash
aws ec2 run-instances --image-id ami-43a15f3e --region us-east-1 --key dev-key \
--security-groups ssh --instance-type t2.micro \
--iam-instance-profile Name="avatar-image"
```

### How to request credentials with curl
```bash
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/avatar-image
```
```json
{
    "Code" : "Success",
    "LastUpdated" : "2016-06-11T03:44:36Z",
    "Type" : "AWS-HMAC",
    "AccessKeyId" : "ASIAJ5HOGZLCDMXIMARA",
    "SecretAccessKey" : "hPaw27itOOxc2Sq2fZxKKgzM9pbctYg5GjnAsUbI",
    "Token" : "FQoDYXdzECUaDAPhEgucj01B4VLx6SKZA/WqJVcs9JDkV83vL62J9dypgAq-
    LIMu2zGajKfJg/xCc57yOh+yBgWx85XLzpQoQNc5oof5Kd1mxW4mkIGeW9uktKrVI
    +oD1FdgEg4ve9U2irMurcYZxlFA9wiZcb2ohrUWDnRMOvIHJEOV3cK1fG0WLVTsetkFAASmOw8jobQ-
    Viy2eocMnFd3bksriq+oy/khsHpm+7SoAcd2cDfpZmhH+ibIl8+fHWD2iLr5pfvqQ2bYsHKW2o6ROY0H
    +18vRXcDaBU9qnHSKTWlw7P7Vv0zE3Vde1y7AH0XPwYp7sbfeQJiKFWBUhNCUTIsHaYoXp-
    JARjJPRLo1Z+gaWipkLc4NSTNgu3m9mSLtS4JjCEJvfLbJ5sF1XxqYJe-
    peGDspTu4YX4DWvpPQuDkvCDswjgASNDR1cdR+jebYuCPiIIQCiUBZeD
    +EKtp3SxKa13mX5EcrH5OSkcOoOq8m9nKNa9HfljVox8TNcwYIvFLNUvWRUWABPt0MyN2YXvGpAA/
    JqFMHzni25+av7noGL2+UJxAgBC8h3BicZyrN4ouZLuugU=",
    "Expiration" : "2016-06-11T09:55:33Z"
}
```
- ### Fortunately, the authors of the AWS SDKs have decided to make things easy for use using the client libraries
- ### For example, Boto has built-in support for IAM roles
    - ### If connecting an AWS service without specifying any credentials, <b>Boto will check</b> to see wheather it is running on an EC2 instance and <b>wheather this instance has an IAM role</b>

# References
- [Using condition in aws policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html)
- [Saving credentials to aws secrets manager](https://aws.amazon.com/blogs/compute/securing-credentials-using-aws-secrets-manager-with-aws-fargate/)