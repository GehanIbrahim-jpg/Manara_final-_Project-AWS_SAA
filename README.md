# Manara_final_Project-AWS_SAA
Scalable Web Application with ALB and Auto Scaling

 # Project Overview
This architecture outlines the integration of various AWS services to design a resilient, scalable, and secure web application hosting environment. Leveraging AWS not only optimizes performance but also ensures a high degree of reliability and security for web applications.
Solution Architecture Diagram
<img width="3969" height="3640" alt="Blank diagram" src="https://github.com/user-attachments/assets/f9a90f3f-5cb2-4cdc-bf20-1f7543afeb3c" />
This diagram visualizes the complete setup: a multi-tier architecture across 2 Availability Zones, public and private subnets, ALB, ASGs, NAT Gateways, RDS, and monitoring services.


  # Architecture:

1. DNS services with Amazon Route 53: Beyond domain management, Route 53 ensures a smooth domain name system service with health checks, traffic flow, and domain registration capabilities. This ensures users can access your web application reliably.
2. AWS WAF: AWS WAF doesn’t just filter out malicious traffic and offers real-time metrics and logging to ensure visibility into attack patterns.
3. Load balancing with Elastic Load Balancing (ELB): ELB is essential for ensuring application availability. It also integrates seamlessly with other AWS services, like EC2 Auto Scaling, which allows for dynamic adjustment of instance count based on traffic load.
4. Firewalls with security groups: Security groups act as virtual firewalls to control the inbound and outbound traffic. They can be easily configured and can be associated with multiple instances, providing a granular level of security.
5. Caching with Amazon ElastiCache: Utilizing ElastiCache, web applications can significantly improve data retrieval speeds, enhancing overall responsiveness. It supports Redis (for complex data structures) and Memcached (for simpler use cases).
6. Managed database with Amazon RDS: With RDS, not only can you choose from six different database engines, but you also benefit from automated backups, patch management, and seamless scaling capabilities.
7. Monitoring with Amazon CloudWatch: Monitor the health and performance of your resources in real-time. Set alarms, visualize logs, and optimize your application’s performance based on the insights.
8. Amazon EC2 – Hosts the web and application servers
9. Auto Scaling Group (ASG) – Automatically adjusts EC2 capacity
10. IAM – Secures access between services
11. Amazon SNS – Sends alerts and notifications
12. NAT Gateway & Internet Gateway – Provide controlled internet access to public and private resources
13. Amazon VPC – Isolated network with subnets and routing
14. Security Groups – Virtual firewalls controlling inbound and outbound traffic for EC2, RDS, and ALB
15. Amazon VPC – Isolated network with subnets and routing


   # Deployment Setup
1. VPC & Subnets
Create a custom VPC with CIDR (e.g., 172.16.0.0/16)
Create 2 public subnets and 4 private subnets across 2 Availability Zones
2. Internet & NAT Gateways
Attach an Internet Gateway to the VPC
Add 1 NAT Gateway per AZ in public subnets
Update route tables to allow:
Public subnets → Internet Gateway
Private subnets → NAT Gateway
3. Security Groups
Create:
WEB Security Group: Allow HTTP/HTTPS from ALB
APP Security Group: Allow traffic from WEB SG
DB Security Group: Allow traffic from APP SG
4. EC2 Instances
Launch EC2 Instances:
Web Layer in Public Subnets (for static content or load testing)
Application Layer in Private Subnets behind ALB
Attach proper IAM Roles for EC2 access to CloudWatch.
5. Application Load Balancer
Configure ALB across 2 AZs
Register web EC2 instances with ALB target group
ALB forwards traffic to instances via listeners (HTTP/HTTPS)
6. Auto Scaling Group (ASG)
Create launch template/launch configuration
Define scaling policies (based on CPU utilization or target tracking)
Distribute instances across AZs for fault tolerance
7. Amazon RDS
Deploy RDS (MySQL/PostgreSQL) in Multi-AZ with private subnet group
Connect RDS to EC2 app instances using DB Security Group
8. Monitoring & Alerts
Enable CloudWatch metrics and logs for EC2, ALB, and RDS
Create CloudWatch alarms ( CPU > 80%)
Set up SNS topics to send email alerts to administrators
9. DNS Setup with Route 53
Create a hosted zone
Point domain or subdomain to the ALB DNS name
