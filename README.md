AWS EC2 Load Balancer Project Documentation
Project Title

High Availability Web Application Deployment Using AWS EC2 and Application Load Balancer (ALB)

1. Project Objective

The objective of this project is to deploy a highly available web application using multiple EC2 instances behind an AWS Load Balancer. The Load Balancer distributes incoming traffic across multiple servers, ensuring fault tolerance, scalability, and high availability.

2. Architecture Overview
Components
Virtual Private Cloud (VPC)
Public Subnets
Internet Gateway
Security Groups
EC2 Instances
Application Load Balancer (ALB)
Target Group
Route 53 (Optional)
CloudWatch Monitoring
Architecture Diagram
                Internet
                    |
                    |
         Application Load Balancer
                    |
       -------------------------
       |                       |
       |                       |
   EC2 Instance 1         EC2 Instance 2
       |                       |
       -------------------------
                |
             VPC Network
3. Prerequisites

Before starting, ensure the following:

AWS Account
Active AWS Account
IAM User with EC2 and ELB permissions
Knowledge Requirements
Basic Linux commands
AWS EC2 concepts
Networking fundamentals
Resources Required
Resource	Quantity
EC2 Instances	2
Load Balancer	1
Security Groups	2
Target Group	1
VPC	1
4. Step 1: Launch EC2 Instances
Login to AWS Console

Navigate to:

AWS Console → EC2 → Launch Instance

Configure Instance 1
Parameter	Value
Name	WebServer-1
AMI	Amazon Linux 2023
Instance Type	t2.micro
Key Pair	Existing/New
Security Group	Web-SG

Launch the instance.

Configure Instance 2
Parameter	Value
Name	WebServer-2
AMI	Amazon Linux 2023
Instance Type	t2.micro
Key Pair	Existing/New
Security Group	Web-SG

Launch the instance.

5. Step 2: Configure Security Group
Create Security Group

Name:

Web-SG
Inbound Rules
Type	Port	Source
SSH	22	My IP
HTTP	80	Anywhere
HTTPS	443	Anywhere
Outbound Rules

Allow All Traffic

6. Step 3: Install Web Server

Connect to each EC2 instance using SSH.

ssh -i key.pem ec2-user@Public-IP

Update Packages:

sudo yum update -y

Install Apache:

sudo yum install httpd -y

Start Apache:

sudo systemctl start httpd
sudo systemctl enable httpd

Create Sample Page on Server 1:

echo "<h1>Server 1</h1>" | sudo tee /var/www/html/index.html

Create Sample Page on Server 2:

echo "<h1>Server 2</h1>" | sudo tee /var/www/html/index.html

Verify:

http://<instance-public-ip>
7. Step 4: Create Target Group

Navigate:

EC2 → Target Groups → Create Target Group

Configuration
Parameter	Value
Target Type	Instances
Protocol	HTTP
Port	80
VPC	Default/Custom

Click Next.

Register Targets

Select:

WebServer-1
WebServer-2

Click:

Include as pending below

Create Target Group.

8. Step 5: Create Application Load Balancer

Navigate:

EC2 → Load Balancers → Create Load Balancer

Select:

Application Load Balancer
Basic Configuration
Parameter	Value
Name	Web-ALB
Scheme	Internet-facing
IP Type	IPv4
Network Mapping

Select:

VPC
Two Availability Zones
Public Subnets
Security Group

Attach:

Web-SG
Listener
HTTP : 80
Target Group

Select:

Web-Target-Group

Click:

Create Load Balancer
9. Step 6: Verify Health Checks

Navigate:

Target Groups → Targets

Check Status:

Healthy

Expected Output:

WebServer-1   Healthy
WebServer-2   Healthy
10. Step 7: Test Load Balancing

Copy ALB DNS Name:

http://web-alb-123456.ap-south-1.elb.amazonaws.com

Open Browser.

Refresh multiple times.

Expected Output:

Server 1

then

Server 2

depending on load balancing algorithm.

11. Step 8: Configure Auto Scaling (Optional)

Navigate:

EC2 → Auto Scaling Groups
Create Launch Template

Use:

Amazon Linux 2023
t2.micro
Web-SG
Create Auto Scaling Group
Parameter	Value
Desired Capacity	2
Minimum Capacity	2
Maximum Capacity	5

Attach to:

Web-ALB
12. Step 9: Monitoring

Navigate:

CloudWatch

Monitor:

CPU Utilization
Request Count
Healthy Host Count
Network Traffic

Important Metrics:

Metric	Purpose
CPUUtilization	Server load
RequestCount	Traffic
HealthyHostCount	Availability
TargetResponseTime	Performance
13. Testing Scenarios
Test 1: Instance Failure

Stop one EC2 instance.

Expected:

ALB routes traffic to healthy instance only.
Test 2: High Traffic

Use:

ab -n 1000 -c 100 http://ALB-DNS/

Expected:

Traffic distributed across servers.
Test 3: Health Check Failure

Stop Apache service:

sudo systemctl stop httpd

Expected:

Instance marked unhealthy.
14. Troubleshooting
Issue	Solution
Target unhealthy	Check HTTP service
Timeout	Verify Security Group
503 Error	Verify Target Group
No response	Check Listener Rules
SSH failure	Verify Key Pair and Port 22
15. Project Outcome

Successfully deployed a highly available web application using AWS EC2 instances behind an Application Load Balancer. The Load Balancer distributes incoming traffic among multiple servers, improving reliability, scalability, and fault tolerance.
