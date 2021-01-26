# Elastic compute cloud (ec2)
- An instance can be thought of as a virtual server
- Amazon Machin Image (AMI) are the main building block of EC2
- Each availability region (AZ) is located in a physically seperate datacenter within its region
- Instance type differ in the amount of resources allocated to them such as
    - m3.medium: 3.75 GB of memory, 1 vCPU
    - c3.8xlarge: 60GB of memory, 32vCPU
    - General purpose (M4, M3)
    - Burstable performance (T2)
    - Compute optimized (C4, C3)
    - Memory intensive (R3)
    - Storage optimized (I2 for performance, D2 for cost)
    - GPU enabled (G2)

## Storage
- Consists of 2 types;
    - Instance storage: is attached directly to the physical host that runs your instance
    - Elastic Block Storage: is attached over the network, create snapshot
    => Effect disk latency and throughput