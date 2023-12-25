# ☁️ AWS Project - AWS VPC With Public And Private Subnets

## 📝 Lab Overview

In this lab we will create a VPC from scratch in the AWS console using services/components like subnets, EC2 Instances, internet gateways, NAT gateways. We also will test the functionality of our infrastructure once built by SSH using EC2 Connect.


## 📐 VPC Architecture Design

![Screenshot](/img/lab_design.png)


## 👉 Step 1 - Create the VPC

* Open the VPC console
* Click on "Your VPCs" on the left-hand panel
* Click "Create VPC" button in the top right hand corner
* Name the VPC "DemoVPC"
* Set the IPv4 CIDR block to 10.0.0.0/16 (that's 65,536 IP Addresses we are assigning to our VPC)
* Leave everything the same and click "VPC" at the bottom right hand corner. DONE


![Screenshot](/img/create_vpc.png)


## 👉 Step 2 - Create the subnets

As shown on our architectural diagram we're going to create 4 Subnets :

* 1 Public subnet and 1 Private subnet in Availability Zone A
* 1 Public subnet and 1 Private Subnet in availability Zone B

Navigate to the VPC console, click on the left hand panel and click subnet.

💡 PRO TIP: Filter your VPC console with the "DemoVPC" we just created. This hides components already created for your default VPC. Shown in screenshot below.


![Screenshot](/img/filter_vpc.png)



We are creating 4 subnets - 2 Public Subnets in separate Availability Zone (AZ) and 2 Private Subnets in the same separate AZs

1. Open the VPC console
2. Click on "Subnets" on the left-hand panel
3. Click "Create subnet" button in the top right hand corner
4. Click the drop down and select the VPC named "Demo VPC" that we created in Step 1
5. Name Subnet 1 "Public Subnet A"
6. Choose an AZ "a" for whatever region you are within. For example: "us-east-1a"
7. Set the IPv4 CIDR block to 10.0.0.0/24 [10.0.0.0 - 10.0.0.255] (that's 256 IP addresses)
8. Then click the button "Add new subnet" at the bottom left hand corner. We will complete similar steps shown in 5-7 for the other 3 subnets we will create.
9. Name Subnet 2 "Public Subnet B", Choose AZ "b", Set the IPv4 CIDR block to 10.0.1.0/24 [10.0.1.0 - 10.0.1.255], click "Add new subnet" at the bottom left hand corner
1. Name Subnet 3 "Private Subnet A", Choose AZ "a" (same AZ as Subnet 1), Set the IPv4 CIDR block to 10.0.16.0/20 [10.0.16.0 - 10.0.31.255] (that's 4096 IP addresses), click "Add new subnet" at the bottom left hand corner
11. Name Subnet 4 "Private Subnet B", Choose AZ "b" (same AZ as Subnet 2), Set the IPv4 CIDR block to 10.0.32.0/20 [10.0.32.0 - 10.0.47.255]
12. Click the "Create subnet" button in the bottom right hand corner.

### Subnet 1 and 2

![Screenshot](/img/subnet_1&2.png)


### Subnet 3 and 4


![Screenshot](/img/subnet_3&4.png)


## 👉 Step 3 - Create internet gateway and route tables

1. Open the VPC Console
2. Click on "Internet gateways" on the left-hand panel
3. Click "Create internet gateway" button in the top right hand corner
4. Name the internet gateway "Demo Internet Gateway"
5. Click "Create internet gateway" in the bottom right hand corner. DONE



