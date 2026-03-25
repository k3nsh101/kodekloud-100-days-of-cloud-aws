### Task

The Nautilus DevOps team is tasked with enabling internet access for an EC2 instance running in a private subnet. This instance should be able to upload a test file to a public S3 bucket once it can access the internet. To achieve this, the team must set up a NAT Gateway in a public subnet within the same VPC.

1. A VPC named `devops-priv-vpc` and a private subnet `devops-priv-subnet` have already been created.
2. An EC2 instance named `devops-priv-ec2` is already running in the private subnet.
3. The EC2 instance is configured with a cron job that uploads a test file to a bucket `devops-nat-21993` once internet is accessible.

Your task is to:

- Create a public subnet named devops-pub-subnet in the same VPC.
- Create an Internet Gateway and attach it to the VPC.
- Create a route table devops-pub-rt and associate it with the public subnet.
- Allocate an Elastic IP and create a NAT Gateway named devops-natgw.
- Update the private route table to route 0.0.0.0/0 traffic via the NAT Gateway.
- Once complete, verify that the EC2 instance can reach the internet by confirming the presence of the test file in the S3 bucket devops-nat-21993. After completing all the configuration, please wait a few minutes for the test file to appear in the bucket, as it may take 2–3 minutes.

### Solution

- Create a subnet

  <img src="./assets_45/create_pub_subnet_1.png" alt="create subnet" />

  <br />

- Create IGW and Attach it to VPC

  <img src="./assets_45/create_ig.png" alt="create igw" />

  <br />

  ```
  Select IGW -> Actions -> Attach to VPC -> Select VPC
  ```

- Create route table

  <img src="./assets_45/create_pub_rt.png" alt="create route table" />

  <br />

- Associate the route table with public subnet

  <img src="./assets_45/create_pub_subnet_2.png" alt="associate rt with subnet" />

  <br />

- Make the subnet public by adding routing for internet

  <img src="./assets_45/create_pub_subnet_3.png" alt="add routes for internet" />

  <br />

- Create NAT gateway

  ```
  VPC -> Virtual private cloud -> NAT gateways -> Create NAT gateway
  ```

  <img src="./assets_45/create_nat.png" alt="create nat" />

  <br />

- Identify route table associated with the private subnet

  <img src="./assets_45/identify_priv_rt.png" alt="private route table" />

  <br />

- Associate the private route table explicitly with private subnet and update the name (optional)

  <img src="./assets_45/update_priv_rt.png" alt="updated private subnet info" />

  <br />

- Update the private route table to route 0.0.0.0/0 traffic via the NAT Gateway

  <img src="./assets_45/priv_rt_to_nat.png" alt="add routes to NAT" />

  <br />

- Verify by visiting the s3 bucket

  <img src="./assets_45/verify.png" alt="uploaded file to s3" />

  <br />
