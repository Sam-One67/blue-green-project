#  Global EKS Scalability & Compliance Platform (Multi-Region)

---

## Executive Summary
This project is a hands-on implementation of a **Blue-Green Deployment Strategy** on **Amazon EKS**, focused on achieving zero-downtime releases and making rollbacks predictable.

Rather than updating services in place, the system keeps two environments running in parallel — **Blue** (current production) and **Green** (next release). The new version is deployed to Green, tested, and only then promoted by switching traffic at the Kubernetes layer.

> **GitOps Model:** Both infrastructure and application state live in Git and are continuously reconciled with the cluster using ArgoCD.

---

##  Architecture Overview

<img width="1264" height="842" alt="Gemini_Generated_Image_hzlblwhzlblwhzlb" src="https://github.com/user-attachments/assets/d3bd1bd0-3e66-49bc-aa5f-a5a469d825b8" />

The architecture follows an **Immutable Infrastructure** pattern — instead of modifying running resources, new versions are deployed alongside existing ones and then swapped.

### At a glance:
* **Provisioning:** AWS infrastructure (VPC, Subnets, EKS) is provisioned using **Terraform**.
* **Orchestration:** Kubernetes manages workloads across multiple **Availability Zones**.
* **Synchronization:** **ArgoCD** keeps the cluster in sync with this GitHub repository.
* **Networking:** Services and Ingress handle internal and external traffic routing.

---

## 🛠️ Design Approach

### 1. Infrastructure as Code (IaC)
All cloud resources are defined in **Terraform**. This makes it easy to recreate environments and avoids the "it works on my machine" issues.

### 2. Blue-Green Strategy
Two environments are always present:
* **Blue (v1.x):** Live production environment.
* **Green (v2.x):** New version under validation.
* **Traffic Switching:** Handled through **Kubernetes Service Selectors**, so there is no need to restart services or touch load balancers.

### 3. GitOps with ArgoCD
ArgoCD watches this repository and applies changes automatically:
* **Auto-sync:** Keeps deployments consistent.
* **Self-healing:** Corrects manual changes in the cluster.
* **Audit Trail:** Git history acts as a full audit log for every change.

---

## ⚙️ CI/CD Workflow

### Continuous Integration (CI)
**Tool:** Jenkins
1.  Build Docker image from source.
2.  Run vulnerability checks (Image Scanning).
3.  Push image to **Amazon ECR**.
4.  Tag using **Semantic Versioning**.

### Continuous Deployment (CD)
**Tool:** ArgoCD
1.  Developer updates manifests in Git.
2.  ArgoCD detects the drift.
3.  **Green Environment** is deployed automatically.
4.  Smoke tests are performed on the Green pods.
5.  **Traffic Shift:** Switch Service selector from Blue to Green.

---

## Observability
Basic monitoring stack is included to keep track of system health:
* **Prometheus:** Metrics collection.
* **Grafana:** Visualization via dashboards.
* **Alertmanager:** Alerting via **SMTP/Gmail**.

---

## 📂 Repository Structure
```bash
├── app-v1/             # Blue (current production)
├── app-v2/             # Green (next release)
├── k8s/                # Kubernetes manifests
│   ├── deployment-blue.yaml
│   ├── deployment-green.yaml
│   ├── service.yaml    # Controls traffic switching
│   └── ingress.yaml
└── terraform/          # Infrastructure (EKS, networking) 
```

---


<div align="center">

# 👨‍💻 Muhammad Ahmed
### *Cloud & DevOps Engineer*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white)](YOUR_LINK_HERE)
[![Portfolio](https://img.shields.io/badge/Portfolio-Visit-%23000000?style=for-the-badge&logo=vercel&logoColor=white)](YOUR_LINK_HERE)
[![Email](https://img.shields.io/badge/Email-Contact-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:muhammaad.ahmaad123@gmail.com)

---

**AWS • Terraform • Kubernetes • Ansible • Docker**

</div>
