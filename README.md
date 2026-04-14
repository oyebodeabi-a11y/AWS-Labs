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

# Server 1 Connect screen shot
<img width="605" height="265" alt="Image" src="https://github.com/user-attachments/assets/662bfa21-d27b-4d08-8f45-6d83866b05a9" />

# Server 2 Connect screen shot
<img width="617" height="255" alt="Image" src="https://github.com/user-attachments/assets/ed6781c6-4541-4460-9c35-1e15a123f4aa" />

# Code fpr Advance details for Instance creation:
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