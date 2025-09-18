# 3-Tier Architecture on AWS

This project demonstrates the deployment of a **3-Tier Web Application Architecture** on **Amazon Web Services (AWS)**.  
It includes the **Presentation Layer (Web Tier)**, **Application Layer**, and **Database Layer**, hosted in a custom **VPC** with secure networking, auto-scaling, and load balancing.

---

## 📌 Architecture Overview

- **Tier 1 – Web Layer**: Public Subnet, EC2 instances behind an Application Load Balancer (ALB).  
- **Tier 2 – Application Layer**: Private Subnet, EC2 instances for backend processing.  
- **Tier 3 – Database Layer**: Private Subnet, Amazon RDS (MySQL/PostgreSQL) in Multi-AZ deployment.  
- **Networking**: Custom VPC with Internet Gateway, NAT Gateway, Route Tables, and Security Groups.  

---

## 🚀 Steps to Deploy

### 1️⃣ Create a Custom VPC

- Create a **VPC** with CIDR block (e.g., `10.0.0.0/16`).
- Enable **DNS hostname and DNS resolution**.

<img src="images/step1-vpc.png" alt="Step 1 - Create VPC" width="400"/>

### 2️⃣ Create Subnets

- **2 Public Subnets** (for Web/Load Balancer).  
- **2 Private Subnets** (for Application and Database layers).  
- Place subnets across **multiple Availability Zones (AZs)** for high availability.

<img src="images/step2-subnets.png" alt="Step 2 - Create Subnets" width="400"/>

### 3️⃣ Configure Networking Components

- **Internet Gateway (IGW)** → Attach to the VPC for internet access.  
- **NAT Gateway** → Place in a Public Subnet to allow private subnets outbound internet access.  
- **Route Tables**:  
  - Public Route Table → Route `0.0.0.0/0` to IGW.  
  - Private Route Table → Route `0.0.0.0/0` to NAT Gateway.  

<img src="images/step3-networking.png" alt="Step 3 - Networking Components" width="400"/>

### 4️⃣ Configure Security Groups & NACLs

- **Web SG**: Allow HTTP(80), HTTPS(443) from anywhere, SSH(22) from your IP.  
- **App SG**: Allow traffic only from Web SG.  
- **DB SG**: Allow MySQL/Postgres (3306) only from App SG.  

### 5️⃣ Launch EC2 Instances

- Launch **EC2 instances** in Public Subnet for the Web Tier.  
- Launch **EC2 instances** in Private Subnet for Application Tier.  
- Attach appropriate **Security Groups**.  
- Install required software (Apache/Nginx, application code, etc.).  

### 6️⃣ Configure Load Balancer

- Create an **Application Load Balancer (ALB)** in Public Subnets.  
- Target Group → Register Web/App instances.  
- Health checks to ensure traffic is routed to healthy instances.  

### 7️⃣ Set Up Auto Scaling

- Configure **Auto Scaling Group (ASG)** for Web/App EC2 instances.  
- Define scaling policies (CPU-based or request-based).  

### 8️⃣ Configure RDS (Database Tier)

- Launch **Amazon RDS (MySQL/Postgres)** in Private Subnets.  
- Enable **Multi-AZ** for high availability.  
- Store credentials in **AWS Secrets Manager** or **SSM Parameter Store**.  

### 9️⃣ Test the Setup

- Access the application using the **ALB DNS name**.  
- Verify traffic flow:
  - User → ALB → Web Tier → App Tier → Database.  

---

## 🛠️ Tools & Services Used

- **VPC, Subnets, Route Tables, Internet Gateway, NAT Gateway**
- **EC2 (Web & App Tiers)**
- **RDS (Database Tier)**
- **Application Load Balancer**
- **Auto Scaling Group**
- **IAM Roles & Security Groups**

---

## 📂 Project Use Case

This architecture is commonly used in **production-grade web applications** to ensure:

- **Scalability** via Auto Scaling.  
- **High Availability** via Multi-AZ deployment.  
- **Security** via isolated subnets and security group rules.  

---

## 📸 Architecture Diagram

<img src="images/3-tier-image.png" alt="Three Tier Architecture Diagram" width="600"/>
---

## ✅ Future Improvements

- Use **Terraform/CloudFormation** for Infrastructure as Code (IaC).  
- Use **Ansible** for configuration management.  
- Add **CI/CD pipeline** for automated deployments.  
- Enable **CloudWatch monitoring & alarms** for proactive alerts.  

---

## 📌 Author

Developed by **Rushikesh Panchal** – Cloud/DevOps Engineer 🚀
