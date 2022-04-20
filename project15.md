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

Use the instructions in the link below to prepare the three instances

![Instances Configuration file](./Installation.md)

We are setting up these instances with the above instructions:
 Bastion
 Nignix
 Tooling
 Webserver
 
 Create an AMI from each of the configured instances
 ![](create-ami.jpg)
 
 With each AMI, create Launch Template.
 ![](launch-template.jpg)
 
 Repeat for all the AMIs
 ![](all-lt)
