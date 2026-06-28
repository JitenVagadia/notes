# Elastic Compute Cloud
* Iaas
* consists of: EC2 (rent VMs), EBS (virtual storage), ELB (load balance), ASG (auto scale)
* Some EC2 instance types also have storage (HDD/SSD)
* can attach NFS, (EBS & EFS)
* can go inside runnings instances with SSH

## EC2 User Data
* can bootstrap instances using user data script
* runs with root user (of installed OS)

## Security Groups
* Fundamental to Network security in AWS
* Control Inbound/outbound/ports/IP-whitelists/protocols
* Specific to region+VPC
* only has allow rules
* everything inbound is blocked by default, everything outbound is allowed by default
* its rules can reference IP or other security groups

## EBS - Elastic Block Store
* A Network volume we can attach to EC2 Instance while they run
* AZ bound
* can only mount to 1 thing at a given time
  * exception
    * io1/io2 types can be attached to multiple instances in same AZ
    * must use cluster aware file system, XFS, EXT4
* Instance created -> will have root EBS Volume (by default deleted when instance terminates)
* Delete on Termination flag
* EBS Snapshots
  * backup a volume at a point in time, no need to detach before but recommended
  * can copy across regions
  * can make a volume out of it, can archive, delete, has a recycle bin
  * first snapshot -> full data, subsequent -> only changes stored on top of last
  * can share to other accounts, can make public, default private
  * source encrypted -> snapshot encrypted, can share encrypted to other accounts if KMS key using which it was encrypted is shared
  * DLM (Data Lifecycle Manager) -> automate creation/deletion
  * charged per GB-month, has tranfer costs across regions
  * FSR (Fast Snapshot Restore) -> fast restore time, additional hourly charge

## AMI - Amazon Machine Image
* prepackage config/software on top of OS
* boot EC2 instances from AMIs
* public/private/marketplace
* built for a region, copy across
* start ec2 instance, customize, stop, build ami using that, will also create ebs snapshot in the process

## EFS - Elastic File System
* Managed NFS, multi AZ, available, scalable, expensive, pay per use
* can use security group to control access
* only for Linux based stuff, not windows
* supports encryption at rest, using KMS
* scales automatically, 1000+ clients, 10GB/s+
* different performance (general purpose, max IO) / throughput (burst, provisioned, elastic) modes
* different storage tiers (standard/IA/archive)
  * moves files using lifecycle policies, can't do stuff for specific files

## ELB - Elastic Load Balancer
* what is the use?
  * spread load, single point of access, handle downstream(customer) failure, ssl termination, separate traffic, handle stickiness with cookies
* ELB pros
  * AWS guarantees, takes care of upgrades, HA, maintenance, just a few config nobs up to us
  * costs a bit more, save more effort
  * seamless integration with other AWS offerings
* 4 kinds:
  * CLB (classic, old), ALB (application, new), NLB (network, transport layer), GWLB (gateway, network layer)
* CLB - fixed host name, supports tcp,http,https
* ALB
  * http/s, websockets
  * supports routing based on path, host, query param, headers
  * port mapping feature to redirect to dynamic ECS port
  * what to route to? target groups - ec2 instances, ASGs, ECS tasks, lambda, IPs
  * fixed host name
  * if routee wants client info, headers are X-Forwarded-(For/Port/Proto)
* NLB
  * for TCP/UDP traffic
  * high throughput, ultra low latency, used for extreme performance
  * one static ip per AZ, can assign elastic ip
  * target groups - ec2, ip, ALB
* GWLB
  * main use is running network appliances on traffic, firewalls, packet inspectors
  * GENEVE protocol (generic network virtualization encapsulation)
  * target groups - ec2, ip
* sticky sessions
  * possible in CLB/ALB (by headers), NLB (by connection attrs)
  * duration based / application based (custom or normal)
  * reserved cookie names (AWSALB, AWSALBAPP, or AWSALBTG)
* Cross zone load balancing
  * if enabled -> distribute load equally among all instances in all TGs
  * if not there -> distribute load equally among all TGs
* Load balancers use X.509 certs, manage certs using ACM (AWS certs manager)
* mutiple certs possible (just like keystore) for 1 load balancer, clients use SNI, only works for ALB, NLB, not CLB
* connection draining, allow inflight requests to complete, disallow new, when something terminating/unhealthy
* ASGs (Auto Scaling Groups)
  * scale out/in, number of ec2 instances
  * free, pay only for underlying instances
  * min,max, initial capacity
  * you need a launch template, basically everything aws will need to create and configure the instances
    * like security groups, user data script, AMI, instance type, ssh key, iam roles, network subnet info
  * possible to scale based on cloudwatch alarms
  * dynamic (target tracking, step), scheduled (time based), predictive scaling
  * configurable cooldown period after scaling (to avoid cascades)
  * instance refresh: (to update apps, phased rollout, provide min healthy %)
* EC2 Instance Metadata (IMDS)
  * Metadata - info about the instance
    * instance can get to know itself
      * http://169.254.169.254/latest/meta-data from inside
      * IMDSv1 - direct
      * IMDSv2 - more secure, token based
  * userdata - launch script for the instance
* 