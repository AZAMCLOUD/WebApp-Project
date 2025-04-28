# Project Documentation: Multi-Region Scalable Web Application Deployment (DevOps)
# Project Overview
Brief description of the project:

Deployed a highly available Node.js web application across multiple AWS regions.

Implemented automatic scaling, load balancing, and CI/CD pipelines.

Ensured fault tolerance, scalability, and high availability.

# Objectives
Deploy a web application across two or more AWS regions.

Set up Elastic Load Balancers and Auto Scaling Groups.

Enable CI/CD automation using CodeCommit, CodeBuild, CodeDeploy, and CodePipeline.

Monitor infrastructure and application health with CloudWatch.

Achieve fault tolerance through multi-region failover.

# Architecture Diagram
(Insert your architecture diagram here — you can use the one we already created together.)

# AWS Services Used
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

# Instance Deployment 
•	Created a Test Instance in each Regions and installed application dependencies on them. i.e Codedeploy Agent, Apache server, cloudwatch agent. 
•	Created an AMI image of the instance for future deployments.
Used AWS Cloudformation Templates in both regions to:       
•	Create Auto Scaling Groups and Elastic Load Balancers.
•	Launch EC2 instances hosting a simple web application with launch templates.
•	Attach instances to Target Groups and Load Balancers.

# Route 53 Configurations 
•	Created a Hosted Zone
•	Created Health checks for both regions 
•	Added Failover-based A records pointing to ELBs in each region

# CI/CD Pipeline Setup
Configured a CodePipeline that:
•	Pulls Web Application source code from GitHub repository.
•	Deploys the artifact using CodeDeploy (using appspec.yml).
Deployment Workflow
•	Developer pushes code changes to GitHub.
•	CodePipeline triggers automatically.
•	CodeDeploy deploys the new build to EC2 instances across multiple regions.
•	Load balancer routes traffic to healthy instances.
•	CloudWatch monitors the entire system for performance and failures.

# Auto-Scaling Deployment and Logs 
Simulated a CPU spike on an Instance to trigger a cloudwatch alarm which then triggers automatic scaling defined by scaling polices (CPU Utilization)

# Test Failover
•	Simulated a region going down i.e.  EC2 in us-east-1
•	Route 53 routes traffic to us-west-1 ELB automaticall

# Monitoring and Logging
CloudWatch Alarms set on:
•	EC2 CPUUtilization
•	Failed Pipeline Executions
•	Route 53 Health Check Status
Created a Web Application Log Groups for application and system logs.
Route 53 Health Checks to monitor regional endpoints

 # Improvements and Next Steps
•	Set up S3 bucket replication for faster cross-region deployments.
•	Implement blue-green deployments for safer updates.
•	Use AWS Global Accelerator for better global performance.







