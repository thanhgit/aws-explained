# Terraform pattern

## Add a ec2
- Build infrastructure
```text
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 2.70"
    }
  }
}

provider "aws" {
  profile = "default"
  region  = "us-east-2"
}

resource "aws_instance" "example" {
  ami           = "ami-305f7155"
  instance_type = "t2.micro"
}
```
- Commands
```text
// create new configuration
$ terraform init
...
Terraform has been successfully initialized!
...

// return the names of the files it formated
$ terraform fmt
example.tf

// validate configuration file
$ terraform validate
Success! The configuration is valid.

// apply configuration into infrastructure
$ terraform apply

// see resource
# aws_instance.example:
resource "aws_instance" "example" {
    ami                          = "ami-305f7155"
    arn                          = "..."
    associate_public_ip_address  = true
    availability_zone            = "us-east-2a"
    cpu_core_count               = 1
    cpu_threads_per_core         = 1
    disable_api_termination      = false
    ebs_optimized                = false
    get_password_data            = false
    hibernation                  = false
    id                           = "i-..."
    instance_state               = "running"
    instance_type                = "t2.micro"
    ipv6_address_count           = 0
    ipv6_addresses               = []
    monitoring                   = false
    primary_network_interface_id = "eni-..."
    private_dns                  = "ip-....us-east-2.compute.internal"
    private_ip                   = "..."
    public_dns                   = "ec2-....us-east-2.compute.amazonaws.com"
    public_ip                    = "..."
    secondary_private_ips        = []
    security_groups              = [
        "default",
    ]
    source_dest_check            = true
    subnet_id                    = "subnet-..."
    tenancy                      = "default"
    volume_tags                  = {}
    vpc_security_group_ids       = [
        "sg-...",
    ]

    credit_specification {
        cpu_credits = "standard"
    }

    metadata_options {
        http_endpoint               = "enabled"
        http_put_response_hop_limit = 1
        http_tokens                 = "optional"
    }

    root_block_device {
        delete_on_termination = true
        device_name           = "/dev/sda1"
        encrypted             = false
        iops                  = 100
        volume_id             = "vol-..."
        volume_size           = 30
        volume_type           = "gp2"
    }
}

// managing state
$ terraform state list
```

## Change infrastructure
- Change *.tf file
- terraform apply

## Destroy infrastructure (terminate resource in your terraform configuration)
- terraform destroy

## How to use variable
- variables.tf
```text
variable "region" {
  default = "us-east-2a"
}

variable "default_volume_size" {
  default = 30
}
```
- sensitive.tfvars
```text
region="us-east-1"
```
- ENV
```text
export TF_VAR_region="us-east-1"
unset TF_VAR_region
```
- Commands
```text
$ terraform apply -var="default_volume_size=40"
$ terraform apply -var-file="sensitive.tfvars"
```

## Random computing
```text
provider "aws" {
  region = "us-east-1"
}

resource "random_integer" "rand" {
  min = 1000
  max = 9999
}

resource "aws_s3_bucket" "bucket" {
  name = "unique-name-${random_integer.rand.result}"
}
```

## Alias command
```text
provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  region = "us-west-2"
  alias = "west"
}

resource "aws_ec2_instance" "example_west" {
  name = "instance2"
  provider = aws.west
}
```

