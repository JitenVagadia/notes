# Docker containers management in AWS
* ECS (AWS's own container platform), EKS (AWS Managed kubernetes), Fargate (serverless container platform), ECR (registry)
## ECS
* EC2 launch type
  * provision and maintain ec2 instances
  * instance runs ecs agent to register in ecs cluster
  * ecs takes care of starting stopping instances
* Fargate launch type
  * no infra provision, serverless!
* IAM Roles for ECS
  * need 2 things, EC2 instance profile if EC2 launch type (wrapper on a IAM role, IMDS stuff) and ECS task role
    * EC2 instance profile (needed for creating cluster): ECS agent inside any ec2 instance uses it to do ecs stuff, (AmazonEC2ContainerServiceforEC2Role managed policy we can attach to role)
    * ECS task role (needed when creating task definitions): what our docker app actually needs
* can mount EFS on ECS tasks
* Amazon ECS Auto Scaling uses AWS Application Auto Scaling
* ILL BE BACK, THIS SHIT HARD :(
* 