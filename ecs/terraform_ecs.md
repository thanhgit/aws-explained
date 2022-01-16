# Overview ECS (Elastic container service)

## Create ECS cluster
```text
resource "aws_ecs_cluster" "my_ecs_cluster" {
    name = "my_ecs_cluster"
}
```

## An autoscaling group launch EC2 instances that will join this cluster:
```text
resource "aws_launch_configuration" "my_launch_configuration" {
    name_prefix = "ecs_launch_config"
    image_id = "${lookup(var.AMIS, var.AWS_REGION)}"
    instance_type = "${var.ECS_INSTANCE_TYPE}"
    key_name = "${aws_key_pair.my_key.key_name}"
    iam_instance_profile = "${aws_iam_instance_profile.ecs-ec2-role.id}"
    security_groups = ["${aws_security_group.ecs_sg.id}"]
    user_data = "/!bin/bash\necho 'ECS_CLUSTER=my_ecs_cluster'>>/ect/ecs/ecs.config\nstart ecs"
    lifecycle {
        create_before_destroy = true
    }
}

resource "aws_autoscaling_group" "ecs_autoscaling" {
    name = "ecs_autoscaling"
    vpc_zone_identifier = ["${aws_subnet.public_zone.id},${aws_subnet.private_zone.id}"]
    launch_configuration = ${aws_launch_configuration.my_launch_configuration.name}
    min_size = 1
    max_size = 1
    tags = {
        key = "Name"
        value = "ecs-ec2-container"
        propagate_at_launch = true
    }
}   
```