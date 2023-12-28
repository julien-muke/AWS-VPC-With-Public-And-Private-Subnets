# ☁️ AWS Project - AWS VPC With Public And Private Subnets

## 📝 Lab Overview

In this lab we will create a VPC from scratch in the AWS console using services/components like subnets, EC2 Instances, internet gateways, NAT gateways. We also will test the functionality of our infrastructure once built by SSH using EC2 Connect.


## 📐 VPC Architecture Design

![Screenshot](/img/lab_design.png)


## 👉 Step 1 - Create the VPC

* Open the VPC console
* Click on "Your VPCs" on the left-hand panel
* Click "Create VPC" button in the top right hand corner
* Name the VPC `DemoVPC`
* Set the IPv4 CIDR block to `10.0.0.0/16` (that's 65,536 IP Addresses we are assigning to our VPC)
* Leave everything the same and click "VPC" at the bottom right hand corner. DONE


![Screenshot](/img/create_vpc.png)


## 👉 Step 2 - Create the subnets

As shown on our architectural diagram we're going to create 4 Subnets :

* 1 Public subnet and 1 Private subnet in Availability Zone A
* 1 Public subnet and 1 Private Subnet in availability Zone B

Navigate to the VPC console, click on the left hand panel and click subnet.

💡 PRO TIP: Filter your VPC console with the `DemoVPC` we just created. This hides components already created for your default VPC. Shown in screenshot below.


![Screenshot](/img/filter_vpc.png)



We are creating 4 subnets - 2 Public Subnets in separate Availability Zone (AZ) and 2 Private Subnets in the same separate AZs

1. Open the VPC console
2. Click on "Subnets" on the left-hand panel
3. Click "Create subnet" button in the top right hand corner
4. Click the drop down and select the VPC named `DemoVPC` that we created in Step 1


![Screenshot](/img/demo_vpc.png)


5. Name Subnet 1 `Public-Subnet-A`
6. Choose an AZ "a" for whatever region you are within. For example: "us-east-1a"
7. Set the IPv4 CIDR block to `10.0.0.0/24` [10.0.0.0 - 10.0.0.255] (that's 256 IP addresses)
8. Then click the button "Add new subnet" at the bottom left hand corner. We will complete similar steps shown in 5-7 for the other 3 subnets we will create.
9. Name Subnet 2 `Public-Subnet-B`, Choose AZ "b", Set the IPv4 CIDR block to `10.0.1.0/24` [10.0.1.0 - 10.0.1.255], click "Add new subnet" at the bottom left hand corner
1. Name Subnet 3 `Private-Subnet-A`, Choose AZ "a" (same AZ as Subnet 1), Set the IPv4 CIDR block to `10.0.16.0/20` [10.0.16.0 - 10.0.31.255] (that's 4096 IP addresses), click "Add new subnet" at the bottom left hand corner
11. Name Subnet 4 `Private-Subnet-B`, Choose AZ "b" (same AZ as Subnet 2), Set the IPv4 CIDR block to `10.0.32.0/20` [10.0.32.0 - 10.0.47.255]
12. Click the "Create subnet" button in the bottom right hand corner.

### 👉 Subnet 1 and 2

![Screenshot](/img/public_sn_1&2.png)


### 👉 Subnet 3 and 4


![Screenshot](/img/private_sn_3&4.png)


## 👉 Step 3 - Create Internet Gateway and Route Tables

1. Open the VPC Console
2. Click on "Internet gateways" on the left-hand panel
3. Click "Create internet gateway" button in the top right hand corner


![Screenshot](/img/internet_GW.png)


4. Name the internet gateway "Demo Internet Gateway"
5. Click "Create internet gateway" in the bottom right hand corner.


![Screenshot](/img/create_internet_GW.png)



### 👉 Attach the Internet Gateway to "Demo VPC"

* Go back to the internet gateway dashboard
* Select the newly created internet gateway called "Demo Internet Gateway"
* Click the "Actions" dropdown button at the top right hand corner and click "Attach to VPC"


![Screenshot](/img/attach_vpc.png)


* Select the "DemoVPC" in the drop down and click "Attach internet gateway" button at the bottom right hand corner


![Screenshot](/img/attach_vpc2.png)



### 👉 Create the Route Tables

A default route table has already been created for our created VPC. However, we will not use the default route table associated with our VPC. We will 3 route tables of our own (Public Route Table, Private Route Table A, Private Route Table B)


* Open the VPC Console
* Click on "Route tables" on the left-hand panel
* Click "Create route table" button in the top right hand corner


![Screenshot](/img/route_tables.png)


* Name the 1st route table `Public-Route-Table`, select `DemoVPC`, click "Create route table" button at the bottom right hand corner.


![Screenshot](/img/create_route_table.png)



* Name the 2nd route table `Private-Route-Table-A`, select `DemoVPC`, click "Create route table" button at the bottom right hand corner.


![Screenshot](/img/private_route_tableA.png)



* Name the 3rd route table `Private-Route-Table-B`, select `DemoVPC`, click "Create route table" button at the bottom right hand corner.



![Screenshot](/img/private_route_tableB.png)



### 👉 Assign Subnets to Route Tables

* Assign Public Subnets to "Public Route Table"
* Go back to the route tables dashboard
* Select "Public Route Table"
* Click "Edit subnet associations" 



![Screenshot](/img/assign_route_table.png)



* Select `Public-Subnet-A` and `Public-Subnet-B` then click "Save associations" button in the bottom right hand corner 



![Screenshot](/img/assign_A&B.png)



### 👉 Assign Private Subnet to "Private Subnet A"

* Go back to the route tables dashboard
* Select `Private-Route-Table-A`
* Click "Edit subnet associations"


![Screenshot](/img/edit_private_RTA.png)



* Select `Private-Subnet-A` and click "Save associations" button in teh bottom right hand corner 


![Screenshot](/img/assign_privateA.png)


### 👉 Assign Private Subnet to "Private Subnet B"

Go back to the route tables dashboard
Select `Private-Route-Table-B`
Click "Edit subnet associations"


![Screenshot](/img/edit_private_RTB.png)


Select `Private-Subnet-B` and click "Save associations" button in teh bottom right hand corner



![Screenshot](/img/assign_privateB.png)



### 👉 Update Public Route Table to make "Public Subnet A" and "Public Subnet B" Public

* Go back to the route tables dashboard
* Select `Public-Route-Table`
* Click "Edit routes"


![Screenshot](/img/edit_route_table.png)


Click "Add route", for Destination route select `0.0.0.0/0`, for Target select `Internet Gateway` and select `Demo-Internet-Gateway`, click "Save changes" button at the bottom right hand corner


![Screenshot](/img/save_route_table.png)



## 👉 Step 4 - Create NAT Gateway

For high availability we will create 2 Create NAT Gateway. One in each AZ (One in Public Subnet A and One in Public Subnet B)

* Open the VPC Console
* Click on "NAT gateways" on the left-hand panel


![Screenshot](/img/create_NAT.png)



* Click "Create NAT gateway" button in the top right hand corner
* Name the 1st Create NAT Gateway `NAT-Gateway-A`, Select `Public-Subnet-A` that we created in a previous step, click "Allocate Elastic IP" button, and click "Create NAT gateway" button in the bottom right hand corner


![Screenshot](/img/save_NAT.png)



* Go back to the NAT gateway dashboard and click "Create NAT gateway" button in the top right hand corner
* Name the 2nd NAT gateway `NAT-Gateway-b`, Select `Public-Subnet-B` that we created in a previous step, click "Allocate Elastic IP" button, and click "Create NAT gateway" button in the bottom right hand corner



![Screenshot](/img/save_NATB.png)



### 👉 Connect Route Tables to NAT gateway

* Open the VPC Console
* Click on "Route tables" on the left-hand panel
* Select `Private-Route-Table-A`
* Click the "Actions" dropdown button at the top right hand corner and click "Edit routes"



![Screenshot](/img/edit_route_table_A.png)



* Click "Add route", for Destination route select `0.0.0.0/0`, for Target select "NAT Gateway" and select `NAT-Gateway-A`, click "Save changes" button at the bottom right hand corner.



![Screenshot](/img/save_route_table_A.png)



* Go back to the Route tables dashboard
* Select `Private-Route-Table-B`
* Click "Actions" dropdown button at the top right hand corner and click "Edit routes"
* Click "Add route", for Destination route select `0.0.0.0/0`, for Target select "NAT Gateway" and select `NAT-Gateway-B`, click "Save changes" button at the bottom right hand corner.



![Screenshot](/img/save_route_table_B.png)



✏️  NOTE: The default NACL allows all inbound and all outbound. We will not change the default NACL settings in this lab. Found in VPC console.



## 👉 Step 5 - Create Bastion Host

✏️ Note: For high availability, it would be best to have one bastion host in each AZ. However, for simplicity, we will only create one Bastion Host - located in Public Subnet A as shown in the "VPC Architecture Design".

* Open the EC2 console
* Click "Instances" on the left-hand panel
* Click "Launch instances" button in the top right hand corner


![Screenshot](/img/instances.png)


* Name the instance `BastionHost`
* Keep AMI as Linux, Keep architecutre 64-bit(x86), Keep instance type t2 micro (to stay within the free tier)
* Key Pair: Select "Proceed without a key pair (Not recommended)" In this lab we will be using the EC2 Connect to SSH into our instance and to test our architecture. Thus, we will not need a key pair here. However, if you want to further secure your instance and if use your own SSH client then a key pair will be needed.
* Edit Network Settings: Change the Default VPC to `DemoVPC`, Change subnet to `Public-Subnet-A`, Enable auto-assign public IP.
* Edit Security Group (SG): Select "Create security group", name the SG `Bastion-Host-SG`, Description: "Security group for Bastion Host" (Optional), Allow SSH from anywhere, 0.0.0.0/0
* Click "Launch instance" button at the bottom right hand corner 



![Screenshot](/img/EC2.jpg)



## 👉 Step 6 - Create Private EC2 Instances

Create Private Insance in Private Subnet A

* Go back to the EC2 console
* Click "Launch instances" button in the top right hand corner
* Name the instance "Private Instance A"
* Keep AMI as Linux, Keep architecutre 64-bit(x86), Keep instance type t2 micro (to stay within the free tier)
* Key Pair: Click "Create new key pair", Name the keypair `VPCKeyPair`, keep everything else default and click "Create key pair" button at the bottom right hand corner - the private key pair file will be downloaded to your computer. Make sure to store this file in a known place on your computuer. We will have to use the contents in this file a later step



![Screenshot](/img/keypair.png)



* Edit Network Settings: Change the Default VPC to `DemoVPC`, Change subnet to `Private Subnet A`, DO NOT enable auto-assign public IP (this is our private instance and it should not have a public IP address)
* Edit Security Group: Select "Create security group", name it `Private-Instance-SG`, Description: "Security group for private instance A and private instance B" (Optional), Allow SSH from Custom: Select the SG of the Bastion Host
Click "Launch instance" button at the bottom right hand corner 



![Screenshot](/img/private_instances1.png)
![Screenshot](/img/private_instances2.png)
![Screenshot](/img/private_instances3.png)
![Screenshot](/img/private_instances4.png)
![Screenshot](/img/save_ec2.png)



Create Private Instance in Private Subnet B

* Go back to the EC2 console
* Click "Launch instances" button in the top right hand corner
* Name the instance "Private Instance B"
* Keep AMI as Linux, Keep architecutre 64-bit(x86), Keep instance type t2 micro (to stay within the free tier)
* Key Pair: In the dropdown select the keypair `VPCKeyPair`
* Edit Network Settings: Change the Default VPC to `DemoVPC`, Change subnet to `Private-Subnet-B`, DO NOT enable auto-assign public IP (this is our private instance and it should not have a public IP address)
* Edit Security Group (SG): Click "Select existing security group", Select the `Private-Instance-SG`,
* Click "Launch instance" button at the bottom right hand corner



## 🏆 Test System

We are going to test our infrastructure to make sure we can properly SSH into our Bastion host and our private EC2 instances. We will also make sure our Bastion host and our private instances have access to the internet via the route tables, the internet gateway, and the NAT gateway (private instances only)

## 👉 Step 7 - SSH into Bastion Host and into Private Instances to Test Connectivity

👉 SSH into our Bastion Host using EC2 Connect

EC2 connect is an AWS feature that allows us to easily and securely SSH into our instances without the need of an external SSH client like Putty

* Open the EC2 console
* Select the `BastionHost`
* Click the "Connect" button at the top right of the page



![Screenshot](/img/connect.png)



* Click "Connect" button at the bottom right of the screen



![Screenshot](/img/connect_instance.png)


* If you get a screen like the one below then you have successfully SSH into your Bastion host



![Screenshot](/img/test1.png)



👉 Test if the Bastion Host has access to the internet

* Type "ping www.google.com" and hit Enter into the terminal window. You should receive feedback as shown in the screen shot below.
* Make sure to hold Ctrl and press "C" to stop the ping.
* If your outcome is similar to what's shown below then your Bastion host has access to the internet 



![Screenshot](/img/test2.png)



👉 SSH into Private Instance A from our Bastion Host

* Type "nano VPCKeyPair.pem" and hit Enter (nano command allows us to create and store a text file within the terminal. We need to upload our VPCKeyPair so we can reference it when we SSH into the Private Instance)



![Screenshot](/img/test3.png)



* Go to the VPCKeyPair.pem file saved on your computer. Open it. Copy ALL the content. Paste it into the terminal (hint hold ctrl and shift and press "v")



![Screenshot](/img/test4.png)



* Hold ctrl and press "X" to exit. Save the content by press Y for yes. Then press "Enter".

Our uploaded VPCKeyPair.pem text file is formated for others to access it (current chmod access code is 644 - "chmod 644"). It is required that your private key files are NOT accessible by others. Therefore, we must change the access permissions for only us to have access ("chmod 400"). For that we will use the "chmod" command

* Type "chmod 400 VPCKeyPair.pem" and press Enter. This remove access for others and only allows read access to us.

* In a seperate tab, go to the EC2 console

* Select "Private instance A" and copy the private IP address as shown in the screenshot below. We will need to reference this IP address to SSH into it. Go back to the EC2 Connect Window terminal.

Now we are ready to SSH into our Private Instance A via our Bastion host by using our keypair

* Type "ssh ec2-user@INSERT YOUR PRIVATE IP ADDRESS YOU JUST COPIED -i VPCKeyPair.pem". Hit Enter. As an example Your code should look like this (your IP address will be different): "ssh ec2-user@10.0.21.1 -i VPCKeyPair.pem"

* A prompt will come up asking if your are sure you want to connect. Type "yes" and you should have successfully SSH into your Private Instance A via your bastion host.



![Screenshot](/img/test5.png)



👉 Test if Private Instance A has access to the internet

* Type "ping www.google.com" and hit Enter into the terminal window. You should receive feedback as shown in the screen shot below.
* Make sure to hold Ctrl and press "C" to stop the ping.
If your outcome is similar to what's shown below then your Bastion host has access to the internet 



![Screenshot](/img/test6.png)


Repeat the Steps 3-4 for Private Instance B if you want to SSH into it and if you want to test if it has access to the internet. NOTE: Remember to use the correct private IP address when executing the "ssh ec2-user@10.0.19.224 -i VPCKeyPair.pem" command.


## 💰 Cost

All services used are eligible for the AWS Free Tier. However, charges will incur at some point so it's recommended that you shut down resources after completing this tutorial.





























