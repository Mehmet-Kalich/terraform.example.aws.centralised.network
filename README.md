# terraform.example.aws.centralised.network

A minimal Terraform example demonstrating how to build a centralised “Hub-and-Spoke” AWS network using:

- **AWS RAM** to share VPCs & subnets from a dedicated **Networking (Hub) Account**  
- **AWS Transit Gateway** to route traffic between the Hub and multiple **Workload (Spoke) Accounts**

---

## 🚀 Overview

As you scale out multiple workload accounts, managing per-account Internet Gateways, NATs, WAFs, etc., quickly becomes costly and complex. This example shows how to:

1. **Create** all network resources (VPC, subnets, IGW, NAT, TGW) in a single **Networking Account**  
2. **Share** the Dev/UAT/Prod VPCs & subnets cross-account via **RAM**  
3. **Attach** shared subnets to an **AWS Transit Gateway** for centralized ingress/egress routing  

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
