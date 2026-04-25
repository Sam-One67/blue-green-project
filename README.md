Enterprise-Grade Kubernetes Orchestration
Multi-Region Blue-Green Deployment Strategy
Executive Summary

This project demonstrates a production-oriented Blue-Green deployment strategy on Amazon EKS, designed to achieve zero-downtime releases and safer rollbacks.

Instead of updating services in place, the system maintains two parallel environments (Blue and Green). At any point, one serves live traffic while the other is used for validation of new releases. Traffic switching is handled at the Kubernetes layer, making deployments predictable and low-risk.

The workflow is fully GitOps-driven, meaning all infrastructure and application states are version-controlled and automatically reconciled with the cluster.

Architecture Overview

The system is based on an immutable infrastructure approach: deployments do not modify running resources but replace them entirely.

At a high level:

AWS infrastructure (VPC, subnets, EKS cluster) is provisioned using Terraform
Kubernetes handles container orchestration across multiple Availability Zones
ArgoCD continuously syncs cluster state with Git
Traffic routing is controlled via Kubernetes Services and Ingress

This setup ensures high availability, reproducibility, and minimal manual intervention.

Key Design Decisions
1. Infrastructure as Code (IaC)

All AWS resources are defined in Terraform. This makes the environment reproducible and avoids configuration drift between environments.

2. Blue-Green Deployment Model

Two separate deployments are maintained:

Blue (v1.x) → current production
Green (v2.x) → new release candidate

Traffic is switched by updating service selectors, avoiding restarts or downtime.

3. GitOps Workflow

ArgoCD monitors this repository and applies changes automatically.

Auto-sync enabled
Self-healing ensures cluster always matches Git
Every deployment is traceable via Git history
CI/CD Pipeline
Continuous Integration (CI)

Tool: Jenkins

On every commit:

Application is built into a Docker image
Image is scanned for vulnerabilities
Image is pushed to Amazon ECR
Semantic versioning is used for tagging

This ensures traceability and consistent builds.

Continuous Deployment (CD)

Tool: ArgoCD

Deployment flow:

Developer pushes manifest changes
ArgoCD detects drift
Green environment is deployed
Smoke testing is performed
Traffic is switched from Blue → Green

Rollback is straightforward: switch traffic back to Blue.

Traffic Management

Traffic routing is handled internally using Kubernetes primitives:

Service selectors act as the traffic switch
Ingress controller exposes the application externally

Switching environments does not require DNS changes or load balancer reconfiguration, which keeps latency effectively unchanged during transitions.

Observability Stack
Prometheus → collects metrics
Grafana → visualizes system health
Alertmanager → sends alerts via SMTP

This provides visibility into deployments, resource usage, and potential failures.

Repository Structure
├── app-v1/                 # Stable version (Blue)
├── app-v2/                 # New release (Green)
├── k8s/                    # Kubernetes manifests
│   ├── deployment-blue.yaml
│   ├── deployment-green.yaml
│   ├── service.yaml        # Traffic switching logic
│   └── ingress.yaml
└── terraform/              # Infrastructure provisioning
Why This Approach

Traditional rolling deployments still carry risk during updates. This setup avoids that by isolating environments completely.

Key benefits:

Zero-downtime deployments
Instant rollback capability
Clear separation between stable and candidate releases
Fully auditable deployment history<img width="1264" height="842" alt="Gemini_Generated_Image_hzlblwhzlblwhzlb" src="https://github.com/user-attachments/assets/4717cc1f-671b-4159-a91d-9b9914c3e557" />
