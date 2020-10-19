# Terraform common

## 4 main data type
- Int - port = 22
- String - host = "localhost"
- List - security_groups = ["abc", "dad"]
- Bool - enabled = true
## Supported types
- Strings
- Maps
- Lists
- Booleans
```text
variable "key" {
    type = "string"
    default = "value"
}

variable "multilineString" {
    type = "string"
    default  = <<EOF
        Line1 
        Line2
    EOF
}
```

## Resource are a component of your infrastructure.
- For example: physical server, virtual machine, NIC, container, ...
- Resources have a set of arguments
    - Required
    - Optional 
- Resources can have exported attributes 
```text
resource "aws_s3_bucket" "my_bucket" {
    bucket = "my-bucket-for-storage"
}
```

## Interpolation syntax
- To reference an exported attribute from a resource 
```text
"${resource_type.resource_name.exported_attributed}"
```
```text
resource "aws_iam_policy" "my_bucket_policy" {
    name = "my_bucket_policy"
    policy = <<POLICY
    {
        "Version": "2020-10-7",
        "Statement": [
            "Action": [
                "s3:ListBucket"
            ],
            "Effect": "Allow",
            "Resource": [
                "${aws_s3_bucket.my_bucket.arn}"
            ]
        ]
    }
    POLICY
}
```

## Data source
- Allow data to be fetched or computed from external resources
- Such as another terraform project or a resource that already exists on AWS
```text
data "aws_db_instance" "database" {
    db_instance_identifier = "my-test-database"
}
```
- The data source above returns many attributes about the database, such as the address of the instance, availability zone or instance size 
- Using "${data.resource_type.resource_name.exported_attribute}"
```text
data "aws_s3_bucket" "my_bucket" {
    bucket = "my_bucket"
}

=> "${data.aws_s3_bucket.my_bucket.arn}"
```

## Terraform locals
- allow you to assign a name to an expression, you can think of them like a variable
- can use a local value multiple times within a module
```text
locals {
    bucket_name_prefix = "seward-"
}

locals {
    default_instance_tag = "my-instance"
}

=> "${local.variable_name}"
```
```text
locals {
    first = "Sward"
    last = "Nguyen"
    full_name = "${local.first}-${local.last}"
}

locals {
    bucket_arn = "${aws_s3_bucket.my_bucket.arn}"
}
```

## Output 
- print the values which is important 
- Output can also be used to return values from a module 
```text
output "ip_public_server1" {
    value = "${aws_instance.server1.ip_public_address}"
}

output "my_value" {
    value = "${local.bucket_name}"
}
```





























