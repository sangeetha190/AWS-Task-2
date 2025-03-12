## Step-by-Step Implementation
1. Create a VPC
 - Navigate to AWS Console → VPC.
 - Click on Create VPC.
 - Enter the following details:
    - Name: MyVPC
    - IPv4 CIDR Block: 10.0.0.0/16
    - Tenancy: Default
    - Click Create VPC.
    ![image](https://github.com/user-attachments/assets/070f9e5b-7564-4634-9da9-e38ffd7037a3)
2. Create Public and Private Subnets
  - Go to VPC Dashboard → Subnets.
  - Click Create Subnet.
  - Select VPC: MyVPC.
     - Create two subnets:
     - Public Subnet
     - Name: MyPublicSubnet
     - CIDR Block: 10.0.1.0/24
     - Availability Zone: Select any AZ
       ![image](https://github.com/user-attachments/assets/6b9e3fcd-36f3-421b-b92c-a9aa16450c76)

  - Private Subnet
    - Name: MyPrivateSubnet
    - CIDR Block: 10.0.2.0/24
    - Availability Zone: Same or different AZ
    - Click Create Subnet.
      ![Screenshot 2025-03-12 111200](https://github.com/user-attachments/assets/210e4093-3ccf-4159-8667-a6f8c2203703)

3. Create Route Tables
  - Navigate to Route Tables.
  - Click Create Route Table.
  - Select VPC: MyVPC.
  - Enter Route Table Name: PublicRouteTable.
  - Click Create.
    ![image](https://github.com/user-attachments/assets/d37d9790-39ab-4534-b0f7-729c9a20d303)
    ![image](https://github.com/user-attachments/assets/c73c999c-62bc-467b-9cb6-fd4fb61c898b)

  - Select PublicRouteTable, go to Routes, click Edit Routes, and add:
    - Destination: 0.0.0.0/0
    - Target: Internet Gateway (IGW)
    - Click Save Routes.
    ![image](https://github.com/user-attachments/assets/9a81b9ce-b87b-4468-84da-5b51cb1c92a7)
 - Associate this route table with PublicSubnet under the Subnet Associations tab.
 - For Private Route Table:
 - Repeat steps 2-5, but name it PrivateRouteTable.
 - ![image](https://github.com/user-attachments/assets/1bbaa245-4137-42f7-a813-6598c7f8ca00)
 - ![image](https://github.com/user-attachments/assets/c93e09b8-22d0-425e-a8cd-c221a54a1685)
 - Do not add an internet route; just keep the default route.
 - Associate this route table with PrivateSubnet.
4. Create an Internet Gateway (IGW)  
  - Go to Internet Gateways.
  - Click Create Internet Gateway.
  ![image](https://github.com/user-attachments/assets/ef0c7d1e-f3d6-4f66-8600-c58dfdc76e5b)
  ![image](https://github.com/user-attachments/assets/9bff4e59-bea0-439a-a5e7-1ee30679ecf9)
  - Enter Name: MyIGW.
  - Click Create and then Attach to VPC → Select MyVPC.
  ![image](https://github.com/user-attachments/assets/7beeedd7-7fe1-4279-974c-c8fe19a72425)
  - Confirm attachment.
5. Security Group Name:
   - Name: PublicEC2Security(Note: The name cannot be edited after creation.)
   - Description: PublicEC2Security
   - Select the VPC: vpc-04077d3839efc13fe (MyVPC)
   - Inbound Rules:
     - Add Rule:
     - Type: SSH
     - Protocol: TCP
     - Port Range: 22
     - Source: Anywhere-IPv4 (0.0.0.0/0)(This allows SSH access from anywhere.)
     - Optional:
       - You can click Add rule if you want to add additional rules (e.g., HTTP on port 80).
     - Click Create security group.
     ![image](https://github.com/user-attachments/assets/8ee1bfbd-acf1-404b-a66b-f0c93b93567a)

   - Name: PrivateEC2Security(Note: The name cannot be edited after creation.)
   - Description: PrivateEC2Security
   - Select the VPC: vpc-04077d3839efc13fe (MyVPC)
   - Inbound Rules:
     - Add Rule:
     - Type: SSH
     - Protocol: TCP
     - Port Range: 22
     - Source: Anywhere-IPv4 (0.0.0.0/0)(This allows SSH access from anywhere.)
     - Optional:
       - You can click Add rule if you want to add additional rules (e.g., HTTP on port 80).
     - Click Create security group.
    ![image](https://github.com/user-attachments/assets/cbb07a74-8d40-4c4b-9d4b-e3cb74fcbe61)

5. Launch EC2 Instance in Public Subnet
  - Navigate to EC2 → Instances → Launch Instance.
    - Choose Amazon Linux 2 as the AMI.
    - Select an Instance Type (e.g., t2.micro).
    - ![image](https://github.com/user-attachments/assets/7ecc565b-de67-4cda-8f45-d0f4cf427b72)
    - Configure Network Settings:
    - VPC: MyVPC
    - Subnet: MyPublicSubnet
    - Auto-assign Public IP: Enable
    - Add Storage, Tags, and Security Group (allow SSH 22, HTTP 80 if needed).
    - Select Existing Key Pair or Create a New One.
    - Click Launch Instance.
    - ![image](https://github.com/user-attachments/assets/6a2e5e35-383e-4b95-adb0-1ae1446d000e)
    - Navigate to EC2 → Instances → Launch Instance.
    - Choose Amazon Linux 2 as the AMI.
    - Select an Instance Type (e.g., t2.micro).
    - ![image](https://github.com/user-attachments/assets/af40f801-7a91-4260-be5a-b3ede725eb08)
    - Configure Network Settings:
    - VPC: MyVPC
    - Subnet: MyPrivateSubnet
    - Auto-assign Public IP: disable
    - Add Storage, Tags, and Security Group (allow SSH 22, HTTP 80 if needed).
    - Select Existing Key Pair or Create a New One.
    - Click Launch Instance.
    - ![image](https://github.com/user-attachments/assets/e62c510f-d5ba-4382-93aa-2db68953f197)


6. Now Connect to EC2 instance using the putty
   
     -  ![image](https://github.com/user-attachments/assets/dc87fe4e-d0e6-445e-99e9-5b069933fab4)
  
      
7. Connect to the EC2 Instance
    - Open a terminal and run:
    - ssh -i "SangeethaCloud01.pem" ec2-user@<Public-IP>
         - scp → Secure Copy Protocol (SCP) to transfer files.
         - -i SangeethaCloud01.pem → Uses the private key for authentication.
         - SangeethaCloud01.pem → The file to transfer (your SSH key).
         - ec2-user@13.203.65.79:~/ → Destination (your Public EC2 instance).
         -  ![image](https://github.com/user-attachments/assets/ebec60ac-5495-4944-b218-27f49df31ea3)
         - Verify the connection.



7. Verify Networking
    - Check Private IP:
    - hostname -I
    - ![image](https://github.com/user-attachments/assets/c11dc80e-b69c-4842-a813-f3fbbcd44315)




### From VM1 to connect to VM2

## This version makes it clearer that:
   - Public IP instances can be accessed directly.
   - Private subnet instances cannot be accessed directly and require a bastion host (jump server) for connectivity.



