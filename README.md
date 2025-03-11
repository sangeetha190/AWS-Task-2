## Step-by-Step Implementation
1. Create a VPC
 - Navigate to AWS Console → VPC.
 - Click on Create VPC.
 - Enter the following details:
    - Name: MyVPC
    - IPv4 CIDR Block: 10.0.0.0/16
    - Tenancy: Default
    - Click Create VPC.
2. Create Public and Private Subnets
  - Go to VPC Dashboard → Subnets.
  - Click Create Subnet.
  - Select VPC: MyVPC.
     - Create two subnets:
     - Public Subnet
     - Name: PublicSubnet
     - CIDR Block: 10.0.1.0/24
     - Availability Zone: Select any AZ
  - Private Subnet
    - Name: PrivateSubnet
    - CIDR Block: 10.0.2.0/24
    - Availability Zone: Same or different AZ
    - Click Create Subnet.
3. Create Route Tables
  - Navigate to Route Tables.
  - Click Create Route Table.
  - Select VPC: MyVPC.
  - Enter Route Table Name: PublicRouteTable.
  - Click Create.
  - Select PublicRouteTable, go to Routes, click Edit Routes, and add:
    - Destination: 0.0.0.0/0
    - Target: Internet Gateway (IGW)
    - Click Save Routes.
 - Associate this route table with PublicSubnet under the Subnet Associations tab.
 - For Private Route Table:
 - Repeat steps 2-5, but name it PrivateRouteTable.
 - Do not add an internet route; just keep the default route.
 - Associate this route table with PrivateSubnet.
4. Create an Internet Gateway (IGW)  
  - Go to Internet Gateways.
  - Click Create Internet Gateway.
  - Enter Name: MyIGW.
  - Click Create and then Attach to VPC → Select MyVPC.
  - Confirm attachment.
5. Launch EC2 Instance in Public Subnet
  - Navigate to EC2 → Instances → Launch Instance.
    - Choose Amazon Linux 2 as the AMI.
    - Select an Instance Type (e.g., t2.micro).
    - Configure Network Settings:
    - VPC: MyVPC
    - Subnet: PublicSubnet
    - Auto-assign Public IP: Enable
    - Add Storage, Tags, and Security Group (allow SSH 22, HTTP 80 if needed).
    - Select Existing Key Pair or Create a New One.
    - Click Launch Instance.
   
6. Connect to the EC2 Instance
    - Open a terminal and run:
    - ssh -i "SangeethaCloud01.pem" ec2-user@<Public-IP>
    - Verify the connection.
7. Verify Networking
    - Check Private IP:
    - hostname -I
    - Check Public IP:
    - curl -s http://checkip.amazonaws.com

![image](https://github.com/user-attachments/assets/6fc85d08-bfcb-4b32-a39f-064a880fc6c6)
### From VM1 to connect to VM2

## This version makes it clearer that:
   - Public IP instances can be accessed directly.
   - Private subnet instances cannot be accessed directly and require a bastion host (jump server) for connectivity.



