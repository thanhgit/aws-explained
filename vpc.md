# Overview VPC (virtual private cloud)

## Components
- VPC CIDR block: providing IP private 
- Subnet: where the instances can be launched
- Internet gateway: connects the instances to the internet
- Route table: place to specify the allowed routes for outbound network traffic
- Security group: restricts the inbound traffic to the instance. It is stateful => return traffic to the ports in the security group is automatically allowed 
- Network access control list: acts as firewall to restrict access in and out of a subnet 

## Note
- Every AWS region comes with a default VPC and a maximum of 5 custom VPC's can be created per region
- If networking isn't important => deploy to default VPC 

## Terraform resource
- aws_pvc : creating a cidr_block
- aws_subnet : can attached route table and security group. The parameters can be cidr_block, vpc_id (aws_vpc._vpc_name_.id) 
- aws_internet_gateway : providing access to the external network for all the nodes in VPC
- aws_default_route_table : provide a resource to manage a default VPC routing table. The parameters is default_route_table_id, cidr_block and gateway_id ( or nat_gateway_id, network_interface_id)
- aws_security_group : is applied at the instance level. It is used to spefity type of traffic can be allowed to the instance and through which ports. The parameters is aws_vpc.__vpc_name_.id, ingress, egress
- aws_eip : create a elastic IP which can be linked with the instance 
- aws_eip_association: associates the elastic IP with the instance. The parameters is instance_id and association_id of the elastic IP
- aws_instance : create the instance. The parameters is AMI and instance_type