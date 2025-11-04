# AWS-VPC-Public-Private-Subnets-setup

## Architecture Overview
        +-------------------------+
        |        AWS VPC          |
        |     (10.0.0.0/16)       |
        |                         |
        |  +-------------------+  |
        |  |   Public Subnet   |  |
        |  | (10.0.1.0/24)     |  |
        |  +-------------------+  |
        |  +-------------------+  |
        |  |   Private Subnet  |  |
        |  | (10.0.2.0/24)     |  |
        |  +-------------------+  |
        +-------------------------+
        
In this task, I successfully created a custom Virtual Private Cloud (VPC) in AWS to establish a secure, scalable, and isolated network environment for deploying cloud resources.
A VPC (Virtual Private Cloud) acts as a private network within AWS, giving full control over IP address ranges, subnets, route tables, gateways, and network access policies.

Instead of using the default VPC, I configured a custom VPC from scratch to understand how AWS networking works at the foundational level.
This exercise demonstrates how cloud engineers can design a network architecture that separates and protects workloads based on their access level and purpose.

The process involved defining a CIDR block, attaching an Internet Gateway, and verifying connectivity between components.
This setup lays the groundwork for creating public and private subnets, EC2 instances, RDS databases, and load balancers in a controlled network environment.

![AWS VPC](https://github.com/Naveen15github/AWS-VPC-Public-Private-Subnets-setup/blob/e9202cd676a11373490d5ef94472d437591f7be8/Screenshot%20(94).png)

## 🌐 Public Subnet Creation
![AWS VPC](https://github.com/Naveen15github/AWS-VPC-Public-Private-Subnets-setup/blob/e9202cd676a11373490d5ef94472d437591f7be8/Screenshot%20(95).png)
In this step, I created a Public Subnet inside the custom VPC to host resources that require direct internet access, such as web servers or bastion hosts.
The subnet was configured with a CIDR block within the VPC range and placed in a specific Availability Zone for redundancy.

An Internet Gateway (IGW) was attached to the VPC, and the public route table was updated to allow outbound traffic to the internet (0.0.0.0/0).
This route table was then associated with the public subnet to make it internet-accessible.

Finally, I verified the setup by launching an EC2 instance in the public subnet and confirming internet connectivity through a public IP.

## 🔒 Private Subnet Creation
![AWS VPC](https://github.com/Naveen15github/AWS-VPC-Public-Private-Subnets-setup/blob/e9202cd676a11373490d5ef94472d437591f7be8/Screenshot%20(96).png)
Next, I created a Private Subnet to securely host backend resources such as databases, application servers, or internal services that should not be exposed directly to the internet.
This subnet was assigned a unique CIDR block within the same VPC and deployed in a different Availability Zone for high availability.

To enable outbound internet access for software updates and API calls, a NAT Gateway (placed in the public subnet) was configured, and the private route table was updated accordingly.
Unlike the public subnet, instances in the private subnet do not receive public IPs, ensuring full isolation from external networks.

This setup provides a secure and efficient network layer for sensitive workloads and internal communication within the AWS infrastructure.

## 🌉 Internet Gateway Creation – mygate
![AWS VPC](https://github.com/Naveen15github/AWS-VPC-Public-Private-Subnets-setup/blob/e9202cd676a11373490d5ef94472d437591f7be8/Screenshot%20(97).png)

In this step, I created an Internet Gateway named mygate to enable internet connectivity for resources within my VPC.
The Internet Gateway acts as a bridge between the VPC and the public internet, allowing instances in the public subnet to send and receive traffic outside AWS.

After creation, mygate was attached to the VPC to establish the connection.
Next, I updated the public route table to include a route (0.0.0.0/0) that directs outbound internet traffic through mygate.

This setup ensures that EC2 instances within the public subnet can access the internet securely while maintaining control through AWS networking components.

## 🛣️ Route Table Creation – myRT
![AWS VPC](https://github.com/Naveen15github/AWS-VPC-Public-Private-Subnets-setup/blob/e9202cd676a11373490d5ef94472d437591f7be8/Screenshot%20(98).png)

In this step, I created a Route Table named myRT to control the flow of network traffic within my custom VPC.
Route tables in AWS define how traffic is directed between subnets and external networks such as the internet or other VPCs.

After creating myRT, I associated it with the Public Subnet to manage internet-bound traffic.
A new route (0.0.0.0/0) was added to myRT, directing all outbound traffic to the Internet Gateway (mygate), allowing instances in the public subnet to access the internet.

This configuration ensures:

Public Subnet instances have internet access through mygate.

The VPC maintains controlled and secure routing for network traffic.

In a production setup, a separate private route table is typically used for private subnets, routing traffic through a NAT Gateway instead of the Internet Gateway.

## 🔧 Editing Subnet Associations
![AWS VPC](https://github.com/Naveen15github/AWS-VPC-Public-Private-Subnets-setup/blob/e9202cd676a11373490d5ef94472d437591f7be8/Screenshot%20(99).png)

After creating the route table myRT, the next step was to edit subnet associations to link it with the appropriate subnet.
Subnet associations determine which subnet’s traffic is controlled by a specific route table within the VPC.

In this case, I associated myRT with the Public Subnet to ensure all outbound traffic from that subnet is routed through the Internet Gateway (mygate).
This allows instances within the Public Subnet to access the internet while maintaining a clear separation from Private Subnets that use different route configurations.

Steps performed:

Opened the Route Tables section in the AWS VPC console.

Selected myRT from the list.

Chose the “Subnet Associations” tab.

Edited associations and selected the Public Subnet to link with myRT.

Saved the configuration to apply the routing changes.

This association ensures controlled and efficient routing, giving the Public Subnet full internet access through mygate, while keeping Private Subnets isolated and secure.
