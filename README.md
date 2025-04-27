# Project Documentation: Multi-Region Scalable Web Application Deployment (DevOps)
1. Project Overview
Brief description of the project:

Deployed a highly available Node.js web application across multiple AWS regions.

Implemented automatic scaling, load balancing, and CI/CD pipelines.

Ensured fault tolerance, scalability, and high availability.

2. Objectives
Deploy a web application across two or more AWS regions.

Set up Elastic Load Balancers and Auto Scaling Groups.

Enable CI/CD automation using CodeCommit, CodeBuild, CodeDeploy, and CodePipeline.

Monitor infrastructure and application health with CloudWatch.

Achieve fault tolerance through multi-region failover.

3. Architecture Diagram
(Insert your architecture diagram here — you can use the one we already created together.)

4. AWS Services Used
Elastic Load Balancer (ELB): Distributes incoming traffic.

Auto Scaling Group (ASG): Automatically adjusts capacity.

EC2: Hosts the Node.js application.

S3: Stores application artifacts.

CloudFormation: Automates infrastructure deployment.

Route 53: Provides DNS routing and health checks.

IAM: Manages roles and security permissions.

CloudWatch: Logs and monitors metrics.

CodePipeline: Orchestrates the CI/CD pipeline.

CodeBuild: Builds and tests application code.

CodeDeploy: Automates deployment to EC2 instances.

