# ☁️ AWS High-Availability & Secure Network Infrastructure

This repository demonstrates a foundational AWS infrastructure designed for **High Availability (HA)** and **Network Isolation**. The project focuses on setting up a secure environment with segmented subnets, load balancing, and private instance management.

## 🏗️ Solution Architecture

![Architecture Diagram](./aws_uygulama1.png)

### Architectural Components:
* **Custom VPC:** A dedicated network environment with a /16 CIDR block.
* **Multi-AZ Readiness:** Subnets are spanned across two Availability Zones (`eu-central-1a` and `eu-central-1b`) to ensure fault tolerance.
* **Network Segmentation:**
    1.  **Public Subnets:** Hosting the Application Load Balancer, NAT Gateways, and VPN Gateway.
    2.  **Private Subnets:** Reserved for application servers (EC2) with no direct public access.
    3.  **Data Subnets (Pre-configured):** Isolated subnets ready for future database (RDS) deployments.

## 🛠️ Implemented Services & Features

| Service | Implementation Details |
| :--- | :--- |
| **Application LB** | Configured to distribute traffic to instances in the private subnet while performing health checks. |
| **NAT Gateway** | Deployed in public subnets to allow private EC2 instances to fetch updates/patches from the internet. |
| **VPN Gateway (Pritunl)** | Set up in the public subnet to provide a secure tunnel for administrative SSH access to private instances. |
| **Route Tables** | Custom routing configured: Public -> Internet Gateway (IGW), Private -> NAT Gateway. |

## 🚀 Security Strategy

### 1. Inbound Traffic Control
External users can only access the web services via the **Application Load Balancer**. Direct access to the internal servers (EC2) is blocked at the network level.

### 2. Management & SSH Access
There is no "Bastion Host" or public SSH port open. Administrative tasks are performed by connecting to the **Pritunl VPN**, allowing secure access to the internal IP range of the VPC.

### 3. Outbound Security
Instances in the private subnet can access the internet (via NAT Gateway) for necessary outgoing traffic, but they remain unreachable from any unsolicited incoming connections.

## 📈 Next Steps (Roadmap)
- [ ] **Database Integration:** Deploy a Multi-AZ RDS instance within the pre-allocated Data Subnets.
- [ ] **S3 & CloudFront:** Offload static assets to S3 and use CloudFront CDN for global caching.
- [ ] **Infrastructure as Code:** Migrate this manual setup into **Terraform** for automated deployment.

---
*Developed as a part of the LimonCloud Academy training program.*
