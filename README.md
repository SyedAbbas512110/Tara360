## Tara360 - Auto-Healing Web Tier

This repository contains the production-ready Terraform configuration to deploy an immutable, self-healing N+1 web tier behind an Application Load Balancer (ALB) on AWS.

## Architecture Choice
Cloud Provider is  AWS. it is Chosen because AWS Auto Scaling Groups (ASGs) paired natively with Application Load Balancer (ALB) target groups provide an out-of-the-box, reliable "auto-healing" loop based on Layer 7 health checks.
Organised with clean naming conventions, input variables, and strict resource isolation.

## Architectural Diagram
Below is the logical design of the `Tara360` infrastructure:

              [ Internet ]
                   │
                   ▼
       [ Application Load Balancer ]
                   │
     ┌─────────────┴─────────────┐
     ▼                           ▼

[ Availability Zone A ]     [ Availability Zone B ]
┌─────────────────────┐     ┌─────────────────────┐
│  Public Subnet 1    │     │  Public Subnet 2    │
│  ┌───────────────┐  │     │  ┌───────────────┐  │
│  │ Web Instance  │  │     │  │ Web Instance  │  │
│  └───────────────┘  │     │  └───────────────┘  │
└─────────────────────┘     └─────────────────────┘
└─────────────── Auto Scaling Group ───────────────┘

## Core Features Satisfied
Self-Healing: The ASG evaluates instances using ELB health checks. If any single VM is terminated or fails a health check, the platform automatically provisions a replacement instance to maintain desired capacity.
N+1 Capacity: Default traffic is continuously balanced across a minimum of two instances provisioned in distinct Availability Zones to ensure zero downtime if an entire AZ drops.
Containerization:`user_data` executes cloud-init scripts on boot to systematically install Docker and deploy the official Nginx container image.

## How to Run
1. Initialise the working directory and plugins:
   ```bash
   terraform init
   terraform validate
   terraform plan
   
   
   
   