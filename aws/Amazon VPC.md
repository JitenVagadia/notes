# VPC
* Private network for your resources, spans multiple AZs
* subnet - partition network inside vpc
* pubilc/private subnet - accessible/not accessible from internet
* route tables to define access between subnets
* Internet Gateway, public subnets have a route to the IG, attached to VPC
* NAT Gateways (AWS managed), NAT instances (self managed) - allow private subnets to access internet while remaining private, attached to public subnet, doesnt allow inbound
* NACL - firewall to control traffic
  * ip based allow deny rules, subnet level
* SGs control traffic to and from ENI (Elastic Network Interface, basically a virtual network card attached to something)/EC2 instance
  * only allow rules for IP/other SGs
* VPC Flow logs
  * VPC/Subnet/ENI Flow logs, info about ip traffic
  * data can go to S3, cloud watch, kinesis data firehose
* VPC peering
  * connect two VPCs (should not have overlapping CIDR)
  * not transitive
* VPC Endpoints - bad idea to connect to other AWS services via public internet, solution is VPC endpoints
  * Without VPC Endpoint: Private EC2 → NAT Gateway → Internet Gateway → S3 (public endpoint)
    * traffic leaves AWS
  * With VPC Endpoint: Private EC2 → VPC Endpoint → S3 
    * traffic stays within AWS backbone, never hits internet
* Site to site VPN - connection between on prem and AWS, over public internet
* Direct Connect (DX) - physical conn between on prem and AWS, over private network