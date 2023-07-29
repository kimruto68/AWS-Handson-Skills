Step 1: Create a VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16
Step 2: Create Subnets
# Subnet 1
aws ec2 create-subnet --vpc-id YOUR_VPC_ID --cidr-block 10.0.1.0/24
# Subnet 2
aws ec2 create-subnet --vpc-id YOUR_VPC_ID --cidr-block 10.0.2.0/24
Step 3: Create a Security Group
aws ec2 create-security-group --group-name MySecurityGroup --description "SSH Access"
Step 4: Configure Security Group Inbound Rule to Allow SSH
aws ec2 authorize-security-group-ingress --group-id YOUR_SECURITY_GROUP_ID --protocol tcp --port 22 --cidr 0.0.0.0/0
Step 5: Create Network ACLs (NACLs)
aws ec2 create-network-acl-entry --network-acl-id YOUR_NACL_ID --rule-number 100 --protocol tcp --port-range From=22,To=22 --rule-action allow --cidr-block 0.0.0.0/0 --egress false
Step 6: Configure NACL Inbound Rule to Allow SSH
aws ec2 create-network-acl-entry --network-acl-id YOUR_NACL_ID --rule-number 100 --protocol tcp --port-range From=22,To=22 --rule-action allow --cidr-block 0.0.0.0/0 --egress false
Step 7: Create Internet Gateway
aws ec2 create-internet-gateway
Step 8: Attach the Internet Gateway to your VPC
aws ec2 attach-internet-gateway --vpc-id YOUR_VPC_ID --internet-gateway-id YOUR_INTERNET_GATEWAY_ID
Step 9: Create a Route Table
aws ec2 create-route-table --vpc-id YOUR_VPC_ID
Step 10: Add a Route to the Internet Gateway in the Route Table
aws ec2 create-route --route-table-id YOUR_ROUTE_TABLE_ID --destination-cidr-block 0.0.0.0/0 --gateway-id YOUR_INTERNET_GATEWAY_ID
Step 11: Associate Subnets with the Route Table
aws ec2 associate-route-table --subnet-id YOUR_SUBNET_1_ID --route-table-id YOUR_ROUTE_TABLE_ID
aws ec2 associate-route-table --subnet-id YOUR_SUBNET_2_ID --route-table-id YOUR_ROUTE_TABLE_ID
