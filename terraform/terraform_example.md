# Terraform example

```text
module "vpc" {
    source = "terraform-aws-modules/vpc/aws"
    providers {
        aws = aws.west
    }
}
```