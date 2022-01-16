# Overview IAM (identity access management)

## Create IAM administrators group and attach Administrator policy
```text
resource "aws_iam_group" "administrators" {
    name = "administrators"
}
resource "aws_iam_policy_attachment" "administrators-attach" {
    name = "administrators-attach"
    groups = [aws_iam_group.administrators.name]
    policy_arm = "arn:aws:iam::aws:policy/AdministratorAccess"
}
```
or
```text
resource "aws_iam_group_policy" "my_developer_policy" {
    name = "my_administrators_policy"
    group = aws_iam_group.administrators.id
    policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
EOF
}
```

## Create user and attach it to a group
```text
resource "aws_iam_user" "admin1" {
    name = "admin1"
}

resource "aws_iam_user" "admin2" {
    name = "admin2"
}

resource "aws_iam_group_membership" "administrators-users" {
    name = "administrators-users"
    users = [
        aws_iam_user.admin1.name,
        aws_iam_user.admin2.name
    ]
    group = aws_iam_group.administrators.name
}
```

## Create a role and attach to an EC2 instance
```text
resource "aws_iam_role" "s3_my_bucket_role" {
    name = "s3_my_bucket_role"
    assume_role_policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "sts:AssumeRole",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Effect": "Allow",
            "Sid": ""
        }
    ]

}
EOF
}

resource "aws_iam_instance_profile" "s3_my_bucket_role_instance_profile" {
    name = "s3_my_bucket_role_instance_profile"
    role = [aws_iam_role.s3_my_bucket_role]
}

resource "aws_instance" "example" {
    ami = "${lookup(var.AMIS, var.AWS_REGION)}"
    instance_type = "t2.micro"
    subnet_id = aws_subnet.public_zone.id
    vpc_security_group_ids = [aws_security_group.allow_ssh.id]
    key_name = aws_key_pair.my_key.name 
    # role
    iam_instance_profile = "${aws_iam_instance_profile.s3_my_bucket_role_instance_profile.name}"
}
```

## Add some permission using a policy document
```text
resource "aws_iam_role_policy" "s3_my_bucket_role_policy" {
    name = "s3_my_bucket_role_policy"
    role = "${aws_iam_role.s3_my_bucket_role}"
    policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*"
            ],
            "Resource": [
                "arn:aws:s3:::my_bucket-abad1",
                "arn:aws:s3:::my_bucket-abad1/*"
            ]
        }
    ]
}
EOF
}
```

## Create a bucket s3
```text
resource "aws_s3_bucket" "my_bucket" {
    bucket = "my_bucket"
    alc = "private"
    tags = {
        Name = "My_Bucket"
    }
}
```