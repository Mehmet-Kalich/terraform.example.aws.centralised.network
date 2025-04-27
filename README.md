# terraform.example.aws.centralised.network

This is a minimal Terraform example showing how to build a centralised **Hub-and-Spoke** AWS network architecture, designed to counter **network sprawl** as your AWS environment scales.

It uses:
- **AWS RAM (Resource Access Manager)** to share VPCs and subnets from a dedicated **Networking (Hub) Account**  
- **AWS Transit Gateway** to route traffic cleanly between the Hub and multiple **Workload (Spoke) Accounts**

By centralising network ownership and routing, this setup simplifies management, reduces costs, and strengthens security across all accounts.

---

## 🚀 Overview

As you scale out multiple workload accounts, managing per-account Internet Gateways, NATs, WAFs, etc., quickly becomes costly and complex. This example shows how to:

1. **Create** all network resources (VPC, subnets, IGW, NAT, TGW) in a single **Networking Account**  
2. **Share** the Dev/UAT/Prod VPCs & subnets cross-account via **RAM**  
3. **Attach** shared subnets to an **AWS Transit Gateway** for centralised ingress/egress routing  

Result: one central hub for Internet traffic, security inspection, routing and cost optimisation.

---

## ⚙️ Repo Layout

```text
.
├── network_account.tf      # Hub VPC, subnets, IGW, NAT, TGW & RAM shares
├── workload_account.tf     # Spoke VPCs, shared subnets, TGW attachments
├── variables.tf            # Common variable definitions
├── dev.tfvars, uat.tfvars, prd.tfvars        # Example var values (CIDRs, AZ mappings, env names)
└── README.md

```text
---

# 🔧 Prerequisites

- Terraform v1.5 or later
- AWS CLI configured with credentials for:
  - **Networking Account** (Hub)
  - **Workload Accounts** (Spokes: Dev, UAT, PRD, etc.)
- IAM permissions to create and manage:
  - VPCs
  - Subnets
  - Internet Gateway (IGW)
  - NAT Gateway
  - Transit Gateway (TGW) and TGW Attachments
  - Resource Access Manager (RAM) shares
  - Route Tables and Routes

---

## 📖 How It Works

1. **Networking (Hub) Account** provisions:
   - Central VPC, Subnets, Internet Gateway, NAT Gateways
   - A Transit Gateway (TGW) for traffic routing
   - RAM shares for VPCs and subnets to workload accounts

2. **Workload (Spoke) Accounts**:
   - Accept shared VPCs and subnets via RAM
   - Attach to the Transit Gateway for ingress/egress routing
   - Use the shared network resources without owning or modifying them

3. **Traffic Flow**:
   - Outbound traffic from workload accounts routes through shared TGW subnets
   - Then through the central Networking Account's NAT Gateways and Internet Gateway
   - Centralises security inspection, simplifies network management, and optimizes cost
