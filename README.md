# AWS-Labs
This is my AWS Labs where I  showcase various hands-on, simulated cloud environments designed by Amazon. I experiment with AWS services like EC2, S3, and databases without incurring real-world costs or setup effort, often featuring step-by-step guidance. Web Services to provide practical experiences.


# AWS-Labs 1: 
I am employed in a cloud computing company. I have been tasked with the following:

Create my own custom VPC to deploy some resources:

VPC and connect with an internet gateway.

create a public route table

create 2 subnetS: subnet A and subnet B

In subnet A, deploy nginx web server

In subnet B, deploy apache2 webserver

then purge apache2 and install jenkins.

make sure the webservers run successfully.

# CREATING A VPC:

For a VPC to be created generated it must have a cider range CIDR of total 32bit

For VPc we have 10.0.0.0/16

# CREATING A SUBNET:

For subnets we have 10.0.1.0/24 and subsequent subnets we have 10.0.2.0/24


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------

## LOAD BALANCER PROJECT

# Project Background:

An AWS Load Balancer, provided by Elastic Load Balancing (ELB), is a fully managed service that automatically distributes incoming application traffic across multiple targets—such as EC2 instances, containers, and IP addresses—across multiple Availability Zones. 

It ensures high availability, fault tolerance, and seamless scaling for applications.

For this project, the Load Balancer used is Application Load Balancer (ALB): Best for HTTP/HTTPS traffic, operating at Layer 7 to provide advanced routing based on content, URLs, and headers.

# Project task:

If there are  2 servers A and B where traffic routed to each is 70% and 30% respectively, the load balancer is implemented to evenly spread the load at 50% each.

# Issues fixed by introducing the AWS load balancer:

Application performance by increasing response time
Reducing network latency

# Project workflow:

# Create a custom VPC

1. Launch AWS console, Select VPC, VPC only

2. Name "load balancer-vpc"

3. IPV is 10.0.0.0/16

4. All other fields are default

5. Successfully create the VPC.

# Launch Internet Gateway

1. Create Internet gateway

2. Name "load balancer-INTGW"

3. All other fields are default

4. Successfully create the Internet gateway.

5. Click on Attach to VPC

6. Select the created VPC and click attach Internet gateway.

# Create 2 Public subnets

1. Click on subnets

2. Do not touch the existing default subnets

3. Click on create Subnet

4. Select the created VPC, "load balancer-vpc"

5. Create subnet A, "load balancer-pub-subA

6. In IPV4 subnet, CIDR block = 10.0.1.0/24
7. Create subnet A

8. Click on create subnet again

9. Repeat steps 1-4.

10. Create subnet B, "load balancer-pub-subB

11. In IPV4 subnet, CIDR block = 10.0.2.0/24

12. Select Availability zone, different from default AZ for subnet A

13. Create subnet B

# Create Route Tables

1. Create public route tables to connect to IGW

2. Create Route table

3. Create name "load balancer - route table

4. Select the created vpc (from create VPC above)

5. Click on create route table.

6. Click on Edit route

7. Click on Add route

8. Select 0.0.0.0 for internet gateway

9. click on IGW to select created internet gateway "load balancer-INTGW"

10. save changes

11. Click on Subnet Association

12. select subnet A and subnet B and save associations.

# Create EC2 instance (Instance 1)

1. Click launch instance

2. Name the instance "load balancer -server A"

3. Select Ubuntu, leave all default selections

4. Select key pair

5. For Network settings, click Edit and select the created VPC "load balancer-vpc"

6. For subnets, select Subnet A

7. Enable Auto assign Public IP

8. Add security group "load balancer-SG"

9. Select HTTP for port 80, in addition to existing default port 22.

10. Change source type to Anywhere

11. Click on Advanced details and include the user data script (shown below)

12. Click on launch instance

# Create EC2 instance (Instance 2)

1. Click launch instance

2. Name the instance "load balancer-server B"

3. Select Ubuntu, leave all default selections

4. Select key pair

5. For Network settings, click Edit and select the created VPC "load balancer-vpc"

6. For subnets, select Subnet B

7. Enable Auto assign Public IP

8. Add security group "load balancer-SG", select existing security group

9. Select HTTP for port 80, in addition to existing default port 22.

10. Change source type to Anywhere

11. Click on Advanced details and include the user data script (shown below)

12. Click on launch instance

# Launch Instances in Browser

1. Copy the IPv4 for both instances and past into 2 seperate browsers.

# Notes for Browser errors:

1. The first thing to suspect if EC2 is not connecting - check that port 22 is connected

2. If the browser is not connecting - check port 80 or else edit inbound rules.

3. If both are not connected and still not working, run local host or sudo systemctl status

# Create load balancer for EC2

1. Click on create load balancer

2. Click on Application load balancer and create

3. Type the load balancer name "loadbalancer"

4. Select created VPC 

5. Select availibilty zone, select subnet

6. Select created security group

7. click on create Target group (appears in a new browser)

8. Name target group "targetgroup"

9. Select the 2 created instances

10. Click "include a pending below", click on next

11. Create target group.

12. Return to the load balancer browser

13. Refresh the target group, select the created target group

14. Leave all options as default

15. Create Load balancer

16. Go to the created target group, select instances

17. click on register targets

# Check the Load balancer works

1. Copy and paste the DNS  of the  load browser to an internet browser

2. Refresh between the 2 created instance

# Expected result: 

The Browser should flick between the 2 created instance evenly.

# Issues encountered: 

Subnets have tobe  placed in 2 different AZs.

# Resolution:

Select the different Availability zones when creating the 2 instances.

# How to terminate the instances used above.

1. Terminate the Target group first

2. Next, terminate the instances.

To terminate further services

1. First, remove the load balancer

2. Remove the target group

3. Terminate the instance before remove the subnet

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


# Server 1 Connect screen shot
<img width="605" height="265" alt="Image" src="https://github.com/user-attachments/assets/662bfa21-d27b-4d08-8f45-6d83866b05a9" />

# Server 2 Connect screen shot
<img width="617" height="255" alt="Image" src="https://github.com/user-attachments/assets/ed6781c6-4541-4460-9c35-1e15a123f4aa" />

# Code for Advanced details for Instance creation:
#!/bin/bash
apt update
apt install -y apache2
cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to cloud computing  Learning</title>
    <style>
        body {
            background-color: #282c34;
            color: white;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }
        h1 {
            color: #61dafb;
            margin: 0.5em 0;
        }
        p {
            color: #adbac7;
            max-width: 600px;
            text-align: center;
            line-height: 1.6;
        }
        .container {
            text-shadow: 2px 2px 4px #000000;
            box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
            border-radius: 10px;
            padding: 20px;
            text-align: center;
        }
        .vote-button {
            margin: 10px;
            padding: 10px 20px;
            font-size: 16px;
            color: #282c34;
            background-color: #61dafb;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        /* Removed hover effect to make the buttons static */
    </style>
</head>
<body>

<div class="container">
    <h1>Welcome to Cloud computing AWS Series Tutorial</h1>
    <h1>server one for cloud computing technology........</h1>
    <p>This is a beautifully colored HTML landing page for your AWS tutorial series. Enhanced with modern CSS styling for a more attractive and engaging user experience.</p>
    <p>This is the best AWS Tutorial series on the internet; Yes or No?</p>
    <form action="URL_TO_YOUR_BACKEND_ENDPOINT" method="post">
        <button type="submit" name="vote" value="yes" class="vote-button">Yes</button>
        <button type="submit" name="vote" value="no" class="vote-button">No</button>
    </form>
</div>

</body>
</html>
EOF
systemctl start apache2
systemctl enable apache2

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Introduction of a Nat gateway

An AWS NAT Gateway is a managed service that allows instances in a private subnet to connect to the internet, other VPCs, or on-premises networks while preventing external entities from initiating connections with them. 

It acts as a secure, high-availability bridge, translating private IP addresses to a public Elastic IP.

Nat gateway has to be in the public subnet to be able to connect to the internet gateway to access the internet.

<img width="584" height="298" alt="Image" src="https://github.com/user-attachments/assets/3b4ae68d-b78e-47cf-888c-ea05b9d000b6" />



# Using the existing customer VPC, from load balancer project

1. Create a new subnet which is a Private subnet.

2. Create a Nat gateway "loadbalance-NATGW"

3. Select the VPC created. Connectivity type, leave  a Public.

4. Create route table called "private routetable"

5. Edit route table

6. Slect 0.0.0.0, Nat gateway, select create NGW

7. Save changes

8. Click on subnet, Subnet associations

9. Click on the private subnet, save associations

# Create EC2 instances

1. Create EC2 instance

2. Auto sign public IP, select disable. This is because the instane is for a private subnet.

# Access to internet via the public subnet

1. Click on the Instance public serverA

2. Click on SSH client.

3. Copy and paste chmond link to VS code

4. Copy and paste 2nd link to VS code

5. Navigate to download folder where the key pair is saved to access the instance server via VS code.

# To terminate all services used

1. Terminate all instances

2. Dissociate IGW and NATGW in the route tables

3. Delete internet gateway

4. Click on route table, remove routes for nat gateway

5. Click on Nat gateway and delete

6. Delete subnets

7. Click on Internet gateway, detach from VPC and click on delete

8. Delete VPC

9. Delete security groups but not any default SGs. 

