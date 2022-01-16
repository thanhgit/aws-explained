# Elastic Container Service
- ### A CloudFormation stack gets created for the ECS cluster

### Task definition
- ### It is an application template and describes one or more containers
- ### While some attributes or settings are configured at the task level, most of them are configured at the container level
- ### Task size consists of Task memory (GB) and Task CPU (vCPU)
- ### Docker container level memory and CPU settings may optionally be defined, but are overridden by task definition level settings
- ### Two memory limits may be defined at the container level: `Soft limit` and `Hard limit`
    - ### The Soft limit corresponds to the `memoryReservation` attribute at the task level. The sum of container memory reservations (Soft limits) must not exceed the task memory
    - ### The Hard limit corresponds to the `memory` attribute at the task level. The Hard limit for each container must not exceed the memory configured at the task level

### Service
- ### A Service implements a task definition, and defines a desired count for tasks to run for a task definition
- ### Optional features such as `auto scaling` and `load balancing` are configured in the service
- ### A security group is created to allow all public traffic to a service on the container port(s) configured
- ### The Load balancer type is None by default, with the option to select Application Load Balancer

### Cluster
- ### A Cluster in an ECS service is a grouping of one or more container services
- ### A cluster name must be unique within an account
- ### By default, a new VPC is created automatically.
- ### By default, new subnets are created automatically The Subnets setting specifies the ID of the subnet to be used by the 

### Task Group
- ### It is a set of related tasks, and all tasks in the same task group are considered as a set when performing spread placement

### Features:
- ### Networking mode, `awsvpc` , is the only supported mode
- ### Host port mappings are not valid with the Fargate launch type networking mode ( awsvpc )
- ### `ecsTaskExecutionRole` is added for the Fargate launch type to allow for pulling Docker images and sending logs to CloudWatch Logs
- ### Only the `awslogs` log configuration and `awslogs` log driver are supported with CloudWatch Logs
- ### Task placement is not supported
- ### Only Docker images on Docker Hub and Amazon ECR are supported
- ### The `host` and `sourcePath` parameters for volumes are not supported with the Fargate launch type

## Using CloudWatch Logs
- ### Amazon CloudWatch is an AWS service for monitoring logs from AWS resources, including Amazon ECS, EC2, EBS volumes, Elastic Load Balancers, and RDS

### `Log event`
- ### which includes a log message and a timestamp, is a record of some event/activity on aresource or application

### `Log stream`
- ### is a continuous sequence of log events
- ### Each log stream is associated with a single log group, and the log streams in a log group do not have to originate from the same resource or application

### `Log group`
- ### is a group of log streams that share the same retention, monitoring, and access control settings

### `Metric filter`
- ### It filter metric observations from events to generate data points for a CloudWatch metric

### `Retention settings`
- ### It indicate the duration for which the log events are to be kept.

## Log driver
- ### `awslogs-region`: Specifies the Amazon region to which CloudWatch logs are sent such as `ap-northeast-1`
- ### `awslogs-group`: Specifies the log group to which the log streams are to send their logs such as `/ecs/nginx`
- ### `awslogs-stream-prefix`: Specifies a prefix that is to be associated with a log stream. Log streams generated with the Fargate launch type have the following format: `prefix-name/container-name/ecs-task-Id` such as `cms`

## Using Auto Scaling
- ### Amazon ECS supports auto scaling `using an auto scaling policy` that consists of a CloudWatch alarm based on one of the ECS service metrics: CPUUtilization or MemoryUtilization

### Two kinds of auto scaling policies are defined:
- ### Target tracking scaling policies
- ### Step scaling policies

### `Target tracking scaling policies`
- ### It referred to is a target value for a specific metric that CloudWatch metrics monitor such as CPU utilization and memory utilization
- ### The number of tasks are increased/decreased in increments/decrements of 1
- ### Increases in CPU or memory utilization are indicators that the application is in need of more tasks and the service scales out if a target metric is exceeded
- ### Sufficient metric data is a prerequisite for a scaling policy to scale
- ### To scale if insufficient data is available, a step scaling policy should be used

### `Step scaling policies`
- ### A CloudWatch alarm could be set to be invoked when a metric state is INSUFFICIENT_DATA
- ### During high use of an application, these metrics are likely to increase and CloudWatch alarms could be set to increase the number of tasks
- ### During times of low application load, these metrics decrease in value and CloudWatch alarms could be set to decrease the number of tasks

### Creating a CloudWatch alarm
- ### Only two ECS metrics are available for configuring a new alarm in Add policy
    - ### `CPUUtilization`
    - ### `MemorUtilization`
- ### FX: auto scaling is to scale the tasks when MemoryUtilization exceeds 256 MB
- ### Specify the number of consecutive periods after which the Alarm threshold would have been exceeded such as 1', 5', 15' 60'

## Using IAM
- ### ECS makes use of `service-linked roles`, which are special types of roles associated with a service to provide access to the required AWS services without additional configuration
- ### ECS makes use of the `AWSServiceRoleForECS` role to access other AWS services for managing EC2 network interfaces, registering/deregistering instances from a load balancer, and registering targets
- ### An IAM user must be granted permission to create the AWSServiceRoleForECS role
- ### Some of the AWS services that may be required to run Amazon ECS include:
    - ### Calls to Amazon ECR to pull Docker images
    - ### Calls to CloudWatch Logs to store container logs
- ### The `AmazonECSTaskExecutionRolePolicy` policy is provided to grant permissions for using the aforementioned ECS services
- ### The `AmazonEC2ContainerServiceRole` policy may be used to register/deregister container instances with load balancers
- ### The service auto scaling IAM role ( `ecsAutoscaleRole` ) is required to configure auto scaling. An IAM user must add `ecsAutoscaleRole` , which must include the which must include the `AmazonEC2ContainerServiceAutoscaleRole` policy
- ### The `AmazonEC2ContainerServiceforEC2Role` policy is not required with the Fargate launch type, as it is provided for the EC2 launch type only

### When creating an Elastic Load Balancer for an ECS service. We need to add a custom policy to the IAM user so that the IAM user is able to configure an Elastic Load Balancer
```json
{
    "Version": "2012-10-17",
    "Statement":[{
        "Effect": "Allow",
        "Action": "elasticloadbalancing:*",
        "Resource": "*"
    }]
}
```

### Using an Application Load Balancer
- ### An ECS service includes `built-in internal load balancing` (target group) to distribute client traffic between the tasks in a service
- ### Exposing containers directly to the public is not the best security practice

## Using Amazon ECS CLI
- ### It is a command-line tool used to create, update, and monitor ECS clusters and tasks
- ### It supports the Fargate launch type. To create a container application with ECS CLI, Docker Compose (v1 or v2) is required

### Install in ubuntu
```bash
wget https://amazon-ecs-cli.s3.amazonaws.com/ecs-cli-linux-amd64-latest
mv ecs-cli-linux-amd64-latest ecs-cli
sudo mv ecs-cli /usr/bin/
```

### Configuring ECS CLI
- ### Configure an ECS cluster with the Fargate launch type
```bash
ecs-cli configure --cluster cluster_name --default-launch-type launch_type --region region_name --config-name configuration_name
```
```bash
cs-cli configure --cluster hello-world --region us-east-1 --default-launch-type FARGATE --config-name hello-world
```
- ### Configure an ECS CLI profile
```bash
ecs-cli configure profile --profile-name profile_name:(Name of an existing ECS cluster or a new cluster to create) --access-key $AWS_ACCESS_KEY_ID --secret-key $AWS_SECRET_ACCESS_KEY
```
```bash
ecs-cli configure profile --access-key $AWS_ACCESS_KEY_ID --secret-key $AWS_SECRET_ACCESS_KEY --profile-name hello-world
```

### Setting up prerequisites for Fargate
- ### Create a task execution role
    - ### `execution-assume-role.json`
        ```json
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "",
                    "Effect": "Allow",
                    "Principal": {
                    "Service": "ecs-tasks.amazonaws.com"
                },
                    "Action": "sts:AssumeRole"
                }
            ]
        }
        ```
- ### Register the task execution policy
- ### Create the task execution role

### Creating an ECS cluster
```bash
esc-cli up
```

### Creating a compose file
```yaml
version: '2'
services:wordpress:
  image: wordpress
  ports:
    - "80:80"
  logging:
    driver: awslogs
    options:
      awslogs-group: hello-world
      awslogs-region: us-east-1
      awslogs-stream-prefix: wordpress
```

### Configuring ECS specific parameters
- ### `ecs-params.yml` defines resource (CPU and memory) settings for the task to create in addition to setting the Network Mode as awsvpc
```yaml
version: 1
task_definition:
  task_execution_role: ecsExecutionRole
  ecs_network_mode: awsvpc
  task_size:
    mem_limit: 0.5GB
    cpu_limit: 256
run_params:
  network_configuration:
    awsvpc_configuration:
      subnets:
        - "subnet-2c02dd4b"
        - "subnet-f2d50bdc"
      security_groups:
        - "sg-8c7dafc7"
      assign_public_ip: ENABLED
```

### Deploying the compose file to the cluster
```bash
ecs-cli compose service up
```
```bash
ecs-cli compose --project-name hello-world service up --create-log-groups
```

### Listing the running containers on the cluster
```bash
ecs-cli compose --project-name hello-world service ps
```

### Listing the container logs
```bash
ecs-cli logs --task-id 0c23d765-88c5-46cd-a317-9db243590c89 –follow
```

### Scaling the tasks on the cluster
```bash
ecs-cli compose --project-name hello-world service scale 2
```

### Deleting the service and the cluster
```bash
ecs-cli compose --project-name hello-world service downecs-cli compose --project-name hello-world service down
```
```bash
ecs-cli down –force
```