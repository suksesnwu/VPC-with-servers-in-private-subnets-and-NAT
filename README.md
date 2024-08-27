# Building a Production-Ready VPC with Private Subnets in AWS

Creating documentation based on your personal experience with the project is a great way to make it both informative and engaging. Here’s how you can structure your documentation to reflect the steps you took while working on the project, highlighting both technical details and your personal approach.

---

# Building a Production-Ready VPC with Private Subnets in AWS

This documentation outlines the process I followed while setting up a production environment using AWS. The setup involves creating a Virtual Private Cloud (VPC) with public and private subnets across two Availability Zones (AZs), deploying an Auto Scaling group for EC2 instances, and configuring an Application Load Balancer (ALB) to distribute traffic.

## Project Overview

The goal of this project was to create a resilient and secure environment for hosting web applications in AWS. By deploying resources across multiple AZs, I ensured high availability and fault tolerance. The architecture includes:
- **Private subnets** for hosting EC2 instances (servers).
- **Public subnets** for NAT gateways and the ALB.
- **NAT gateways** for allowing outbound internet access from the private subnets.
- **S3 gateway endpoint** for private, efficient access to S3.

### Architecture Diagram

Here’s an overview of the architecture:

![vpc-example-private-subnets](https://github.com/user-attachments/assets/76b0c6c9-00f9-4551-8434-d8f8b131d8d5)


## Steps I Took to Set Up the VPC

### 1. VPC Creation and Subnet Configuration

1. **Create the VPC**:
   - Opened the Amazon VPC console and chose **Create VPC**.
   - Selected **VPC and more** under "Resources to create."
   - Gave the VPC a descriptive name (e.g., "Prod-VPC").
   - Used the default IPv4 CIDR block (or customized based on project needs).

2. **Set Up Subnets**:
   - Chose **2 Availability Zones** for better resilience.
   - Created **2 public subnets** and **2 private subnets**.
   - Customized the CIDR blocks to ensure no overlap and efficient IP allocation:
     - Public Subnet 1: `10.0.1.0/24`
     - Public Subnet 2: `10.0.2.0/24`
     - Private Subnet 1: `10.0.3.0/24`
     - Private Subnet 2: `10.0.4.0/24`

3. **NAT Gateways**:
   - Configured 1 NAT gateway per Availability Zone to ensure high availability.
   - Attached the NAT gateways to the public subnets.

4. **VPC Endpoint for S3**:
   - Chose to add a gateway VPC endpoint for S3 access from private subnets.

### 2. Deploying the Application

Once the VPC was set up, I moved on to deploying the application using EC2 instances managed by an Auto Scaling group:

1. **Create a Launch Template**:
   - Defined the EC2 instance configuration using a launch template.
   - Specified instance type, AMI, and user data scripts for automated setup.
   - Configured security groups to allow only necessary inbound traffic (e.g., HTTP, SSH).

2. **Set Up Auto Scaling Group**:
   - Created an Auto Scaling group across both private subnets.
   - Set the desired, minimum, and maximum instance counts based on expected load.
   - Ensured the group was set to scale up or down automatically based on CPU utilization.

3. **Configure the Application Load Balancer**:
   - Created an ALB and linked it to the Auto Scaling group.
   - Set up listeners and target groups to route traffic to the EC2 instances in the private subnets.
   - Configured health checks to monitor instance health.

### 3. Testing and Validation

After deploying the application, I conducted thorough testing to ensure everything worked as expected:

1. **Connectivity Tests**:
   - Verified that the ALB was correctly routing traffic to healthy instances.
   - Ensured that the instances could access the internet for updates using the NAT gateway.

2. **Troubleshooting with Reachability Analyzer**:
   - Used AWS Reachability Analyzer to diagnose any potential network misconfigurations, such as incorrect route tables or security group settings.

3. **S3 Access from Private Subnets**:
   - Tested the connection to S3 using the VPC endpoint, ensuring data transfer was efficient and secure.

## Lessons Learned and Best Practices

Throughout this project, I learned several key things:
- **Planning CIDR blocks**: Proper planning of CIDR blocks is crucial to avoid IP conflicts and allow for future scalability.
- **High Availability**: Deploying resources across multiple AZs greatly improves resilience.
- **Security Considerations**: Always restrict access using security groups and limit public access only to necessary resources.

## Next Steps

Moving forward, I plan to:
- Implement monitoring and alerts using CloudWatch.
- Explore further optimizations for cost management.
- Automate the deployment using Terraform or CloudFormation.

---

### Final Notes

This documentation not only serves as a guide for replicating the setup but also reflects the decisions and challenges faced during the project. I hope it helps others who are looking to build a similar architecture.

---

## Credits

This project is based on the example architecture and steps provided by [AWS](https://aws.amazon.com). Special thanks to the AWS team for offering comprehensive documentation that made this project possible. 

For more detailed information, visit the official AWS documentation: [VPC with Public and Private Subnets (NAT)](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-example-private-subnets-nat.html).

