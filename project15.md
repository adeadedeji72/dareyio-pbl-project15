## AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY ##
The power of Clouds is not only in being able to rent Virtual Machines – it is much more than that. From now on, we will gradually study 
different Cloud concepts and tools on example of AWS, GCP, Azure, etc. The principles are common across most of the major Cloud Providers.

This project builds an AWS VPC (Virtual Private Cloud) network for a fictitious company that uses WordPress CMS for its main business website, and a Tooling Website.
As part of the company’s desire for improved security and performance, a decision has been made to use a reverse proxy technology from NGINX to achieve this.

![](tooling_project_15.png)

All recources are prefixed "masterclass" to delineate resources for this project from those of other projects

Create a VPC
Create 2 Public (Shared across two availability zones within the same region)
Create 4 Private Subnets (Shared across two availability zones within the same region)

![](vpc.jpg)
![](subnets.jpg)

Create Internet Gateway in to Public Subnet 1
Create NAT Gateways in to Public Subnets 1 and 2

Create Security Groups to allow traffic within the infrastructure:
 - [x] **Nginx Servers**: Access to Nginx should only be allowed from a Application Load balancer (ALB).
 - [x] **Bastion Servers**: Access to the Bastion servers should be allowed only from workstations that need to SSH into the bastion servers. Hence, 
  you can use your workstation public IP address.
 - [x] **Application Load Balancer**: ALB will be available from the Internet
 - [x] **Webservers**: Access to Webservers will be allowed from the Nginx servers. 
 - [x] **Data Layer**: Only webservers connect to RDS, while Nginx and Webservers will have access to EFS Mountpoint.

![](security-groups.jpg)
Create EC2 Instances that will form the bases for Bastion, Tooling, Nginx and WordPress AMIs
Launch 3 instances of Red Hat Linux t2.micro
 For Bastion:
  Login to the instance designated for Bastion
  Update the instance
  Install these package:
  ~~~
  sudo yum -y install python ntp net-tools vim wget telnet epel-release htop
  ~~~
  Create an AMI from the instance when the packages are installed 
  
- Prepare Launch Template For Bastion (One per subnet)
Make use of the AMI to set up a launch template
Ensure the Instances are launched into a public subnet
Assign appropriate security group
Configure Userdata to update yum package repository and install Ansible and git
- Configure Target Groups
Select Instances as the target type
Ensure the protocol is TCP on port 22
Register Bastion Instances as targets
Ensure that health check passes for the target group
- Configure Autoscaling For Bastion
Select the right launch template
Select the VPC
Select both public subnets
- Enable Application Load Balancer for the AutoScalingGroup (ASG)
Select the target group you created before
- Ensure that you have health checks for both EC2 and ALB
The desired capacity is 2
Minimum capacity is 2
Maximum capacity is 4
Set scale out if CPU utilization reaches 90%
Ensure there is an SNS topic to send scaling notifications
