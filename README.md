# AWS Auto Scaling Project

This project demonstrates how to build a highly available, scalable, and self-healing web application architecture on AWS using EC2, Auto Scaling Groups, Launch Templates, Application Load Balancers, Custom AMIs, EBS, and IAM.

The infrastructure automatically launches new EC2 instances during increased traffic, removes unhealthy instances, and replaces failed servers without manual intervention.

This architecture represents a production-style cloud environment commonly used by modern web applications.

## Project Objectives
- Build a highly available web infrastructure.
- Implement automatic scaling based on workload.
- Configure self-healing mechanisms.
- Create reusable server templates using AMIs.
- Distribute traffic using an Application Load Balancer.
- Learn cloud scalability and fault tolerance concepts.

## AWS Services Used
- **AWS IAM (Identity and Access Management):** Securely manages access and permissions for AWS resources.
- **Amazon EC2 (Elastic Compute Cloud):** Provides scalable, virtual computing instances (servers) in the cloud.
- **Amazon EBS (Elastic Block Store):** Delivers high-performance, persistent block storage volumes designed for use with EC2 instances.
- **Amazon ALB (Application Load Balancer):** Manages and routes incoming traffic to ensure the application stays continuously running.
- **Security Groups:** Act as a virtual firewall to control inbound and outbound traffic for the EC2 instances.
- **Target Groups:** A logical routing pool that tells the load balancer exactly where to direct incoming traffic (instances, containers, or IP addresses).
- **Launch Templates:** The configuration blueprint for EC2 instances.
- **Auto Scaling Groups (ASG):** Automates instance management, scaling, and self-healing.
- **Amazon Machine Image (AMI):** Serves as the "Golden Image" pre-configured server template.

---

## Core Concepts Explained

### 1. AWS IAM
This service helps you securely control access to AWS resources. It allows you to manage users, roles, and permissions to strictly define who or what can access your AWS environment. IAM is a **global service**, meaning it is not region-specific and is managed centrally.

### 2. Amazon EC2
Amazon EC2 provides virtual machines (instances) in the cloud. Instead of purchasing physical hardware, you can launch these virtual servers in minutes and easily scale them up or down depending on your traffic demand.

### 3. Amazon EBS
EBS is a cloud-based storage service that provides durable, high-performance block storage volumes. It functions like a virtual hard drive attached to a physical server, ensuring data persists independently of the lifespan of the EC2 instance.

### 4. Amazon ALB
An ALB is a traffic controller that automatically distributes incoming web traffic across multiple targets (like EC2 instances) to balance the workload, maximize throughput, and prevent server overloads.

### 5. Security Group
A Security Group acts as a stateful, host-level firewall for EC2 instances, allowing you to set explicit rules to control both inbound and outbound network traffic.

### 6. Target Group
A Target Group acts as a routing destination for the load balancer. Instead of sending traffic directly to individual servers, the load balancer sends traffic to the target group, which then intelligently distributes the requests across the registered health-checked instances.

### 7. Amazon Machine Image (AMI)
An AMI is a pre-configured machine template that packages the operating system, server configurations, applications, and system utilities together. It serves as a blueprint, allowing teams to launch perfectly replicated instances instantly.

### 8. Launch Template
A saved blueprint that stores all the necessary configuration details—such as the AMI, instance type, network settings, and security groups—needed to launch an EC2 instance. It standardizes server setups, making it fast and easy to spin up identical instances manually or automatically via Auto Scaling.

### 9. Auto Scaling Group (ASG)
A service that automatically monitors and adjusts the number of EC2 instances running to handle your application's traffic. It launches new servers during high-demand spikes to prevent crashes and terminates extra servers during low-traffic periods to save costs. It also ensures high availability by automatically replacing any unhealthy or failed instances.

---

## IAM Policies Utilized
* `AdministratorAccess` — Used for initial administrative user and environment setup.
* `AmazonSSMManagedInstanceCore` — Attached to the EC2 instance profile to allow secure Systems Manager access without opening SSH ports (Port 22) to the public.

---

## Architecture
<img width="1623" height="990" alt="AWS Architecture Diagram" src="https://github.com/user-attachments/assets/d5d7ac0b-0fce-47c8-b5bb-4c99d62a7d7c" />

---

## Steps Performed

### Phase 1: Create Golden Image
1. Launch a base EC2 instance.
2. Install Nginx web server.
3. Configure the custom website landing page.
4. Create a custom AMI from the running instance.
5. Verify AMI availability in the AWS console.

### Phase 2: Create Launch Template
1. Create a new Launch Template.
2. Select the custom AMI created in Phase 1.
3. Configure the required instance type (e.g., `t2.micro`).
4. Attach the IAM instance profile/role.
5. Configure the appropriate Security Groups.

### Phase 3: Create Target Group
1. Create a Target Group targeted for "Instances".
2. Configure HTTP health checks.
3. Set the health check path to `/`.

### Phase 4: Create Application Load Balancer
1. Create an internet-facing ALB.
2. Select multiple Availability Zones for high availability.
3. Configure HTTP listeners.
4. Attach the Target Group created in Phase 3.

### Phase 5: Create Auto Scaling Group
1. Create an Auto Scaling Group.
2. Attach the Launch Template created in Phase 2.
3. Select the appropriate public/private subnets.
4. Attach the existing Load Balancer / Target Group.
5. Configure the capacity boundaries (Minimum, Desired, Maximum).

---

## Auto Scaling Configuration

| Configuration Metric | Value | Description |
| :--- | :--- | :--- |
| **Minimum Capacity** | 2 | Ensures high availability even during low traffic periods. |
| **Desired Capacity** | 2 | The standard baseline number of operational instances. |
| **Maximum Capacity** | 4 | The ceiling limit for instances during peak traffic spikes. |

This configuration ensures that at least two web servers are always available across different availability zones while allowing automatic scaling when traffic increases.

---

## Self-Healing Test
1. **Manual Termination:** Manually terminated one operational EC2 instance via the AWS Console.
2. **Detection:** The Auto Scaling Group detected the instance failure via health checks.
3. **Recovery:** ASG automatically launched a new EC2 instance to maintain the desired capacity of 2.
4. **Registration:** The new instance automatically registered with the Load Balancer's Target Group.
5. **Availability:** The website remained fully accessible to end-users throughout the process.

### Result
Successful automatic recovery without any application downtime.

---

## Key Concepts Learned
* Auto Scaling Groups & Scaling Policies
* Launch Templates vs. Launch Configurations
* Application Load Balancers & Target Groups
* High Availability & Fault Tolerance Design
* Self-Healing Infrastructure
* Golden Images (AMI) Creation
* Cloud Scalability & Cost Optimization

## Skills Demonstrated
* **AWS Services:** EC2, ASG, ALB, AMI, EBS, IAM
* **Linux Administration:** Nginx installation and configuration, Bash scripting
* **Architectural Design:** High Availability, Fault Tolerance, Infrastructure Automation

## Conclusion
Successfully designed and deployed a highly available, scalable, and self-healing web architecture on AWS capable of automatically responding to workload changes and infrastructure failures. This project demonstrates production-level cloud infrastructure concepts commonly used in modern DevOps and Cloud Engineering environments.
