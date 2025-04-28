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

![Test Instance with Dependencies Installed](/Screenshots/Screenshots(7).png)


•	Created an AMI image of the instance for future deployments.

Used AWS Cloudformation Templates in both regions to:     

•	Create Auto Scaling Groups and Elastic Load Balancers.

•	Launch EC2 instances hosting a simple web application with launch templates.

•	Attach instances to Target Groups and Load Balancers.

```YAML
AWSTemplateFormatVersion: '2010-09-09'
Description: Deploy EC2 behind ALB with Auto Scaling using an AMI

Parameters:
  VpcId:
    Type: AWS::EC2::VPC::Id
    Description: Select the VPC
  SubnetIds:
    Type: List<AWS::EC2::Subnet::Id>
    Description: Select the subnets
  InstanceRoleArn:
    Type: String
    Description: Enter the IAM Role ARN for the EC2 instance
  SecurityGroupId:
    Type: AWS::EC2::SecurityGroup::Id
    Description: Select a security group for the resources
  AmiId:
    Type: AWS::EC2::Image::Id
    Description: AMI ID to launch EC2 instance

Resources:
  LaunchTemplate:
    Type: AWS::EC2::LaunchTemplate
    Properties:
      LaunchTemplateName: WebAppLaunchTemplate
      LaunchTemplateData:
        ImageId: !Ref AmiId
        InstanceType: t2.micro
        IamInstanceProfile:
          Arn: !Ref InstanceRoleArn
        NetworkInterfaces:
          - AssociatePublicIpAddress: true
            DeviceIndex: 0
            Groups:
              - !Ref SecurityGroupId
        UserData:
          Fn::Base64: !Sub |
            #!/bin/bash
            systemctl start httpd
            systemctl enable httpd
            cat <<EOF > /var/www/html/index.html
            <!DOCTYPE html>
            <html lang="en">
            <head>
              <meta charset="UTF-8">
              <title>Chiazam Iheme - DevOps Engineer</title>
            </head>
            <body>
              <h1>Chiazam Iheme</h1>
              <p>DevOps Engineer</p>
            </body>
            </html>
            EOF

  AppTargetGroup:
    Type: AWS::ElasticLoadBalancingV2::TargetGroup
    Properties:
      Name: WebAppTargetGroup
      VpcId: !Ref VpcId
      Port: 80
      Protocol: HTTP
      TargetType: instance
      HealthCheckPath: /

  ApplicationLoadBalancer:
    Type: AWS::ElasticLoadBalancingV2::LoadBalancer
    Properties:
      Name: !Sub WebAppALB-${AWS::StackName}
      Subnets: !Ref SubnetIds
      SecurityGroups:
        - !Ref SecurityGroupId
      Scheme: internet-facing
      LoadBalancerAttributes:
        - Key: idle_timeout.timeout_seconds
          Value: '60'

  ALBListener:
    Type: AWS::ElasticLoadBalancingV2::Listener
    Properties:
      LoadBalancerArn: !Ref ApplicationLoadBalancer
      Port: 80
      Protocol: HTTP
      DefaultActions:
        - Type: forward
          TargetGroupArn: !Ref AppTargetGroup

  AutoScalingGroup:
    Type: AWS::AutoScaling::AutoScalingGroup
    Properties:
      VPCZoneIdentifier: !Ref SubnetIds
      LaunchTemplate:
        LaunchTemplateId: !Ref LaunchTemplate
        Version: !GetAtt LaunchTemplate.LatestVersionNumber
      MinSize: '1'
      MaxSize: '5'
      DesiredCapacity: '1'
      TargetGroupARNs:
        - !Ref AppTargetGroup
      HealthCheckType: EC2
      HealthCheckGracePeriod: 300
```

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







