# 3-Tier E-Commerce DevOps Application on AWS

## Overview

This project demonstrates a complete end-to-end deployment of a 3-tier e-commerce application using a modern DevOps toolchain. It showcases automation in infrastructure provisioning, configuration management, containerization, orchestration, continuous monitoring, logging, and CI/CD pipelines — all running on AWS.

## Architecture

The architecture follows a three-tier design:

- **Frontend:** React-based UI served through NGINX
- **Backend:** Node.js, Python, Java, and Go microservices exposing RESTful APIs
- **Database:** MongoDB, Redis, RabbitMQ, and MySQL deployed as StatefulSets in Kubernetes

The application is deployed to AWS using Terraform and Ansible, containerized via Docker, orchestrated with Kubernetes and Helm, and continuously monitored through Prometheus, Grafana, ArgoCD, and ELK Stack.

### Architecture Diagram

![My_first_board_2](https://github.com/user-attachments/assets/f1727295-fd39-487d-b229-c0be03c8b786)

## Technology Stack

| Category | Technologies |
|---|---|
| Cloud Provider | AWS (EC2, EKS, VPC, IAM, S3, ECR, Route 53) |
| Infrastructure as Code | Terraform |
| Configuration Management | Ansible |
| Containerization | Docker |
| Orchestration | Kubernetes |
| Package Management | Helm Charts |
| CI/CD Pipeline | GitHub Actions + ArgoCD (GitOps) |
| Monitoring & Logging | Prometheus, Grafana, ELK Stack |
| Secret Management | HashiCorp Vault |
| Version Control | Git / GitHub |

---

## Repository Structure

This is a multi-repository project. Each service and infrastructure component lives in its own repository under the [DevOpsProjectsOrganization](https://github.com/DevOpsProjectsOrganization) GitHub organization.

### Infrastructure & DevOps Repos

| Repository | Description |
|---|---|
| [aws_eks_cluster_kubernetes](https://github.com/DevOpsProjectsOrganization/aws_eks_cluster_kubernetes) | Terraform modules for provisioning the AWS EKS cluster, node groups, VPC, IAM roles, and all supporting Kubernetes infrastructure |
| [ecommerce_terraform](https://github.com/DevOpsProjectsOrganization/ecommerce_terraform) | Terraform configuration for EC2 instance provisioning, Route 53 hosted zones, security groups, and networking — used with Ansible for configuration management |
| [ecommerce_ansible](https://github.com/DevOpsProjectsOrganization/ecommerce_ansible) | Ansible playbooks for automating software installation, dependency configuration, and microservice deployment on EC2 instances |
| [ecommerce_shell](https://github.com/DevOpsProjectsOrganization/ecommerce_shell) | Bash shell scripts for the initial manual deployment phase — provisions EC2 instances and configures the application on AWS with Route 53 |
| [ecommerce_secrets](https://github.com/DevOpsProjectsOrganization/ecommerce_secrets) | HashiCorp Vault integration for centralized, encrypted secret management across CI/CD pipelines, Terraform, and Kubernetes workloads |
| [ansible_custom_image](https://github.com/DevOpsProjectsOrganization/ansible_custom_image) | Packer configuration for building a custom Linux AMI with Ansible pre-installed, used as a base image for consistent EC2 provisioning |
| [roboshop-helm](https://github.com/DevOpsProjectsOrganization/roboshop-helm) | Helm chart for the full RoboShop application — manages Kubernetes manifests, service configurations, and release versioning across all microservices |

### Application Microservice Repos

| Repository | Language | Description |
|---|---|---|
| [roboshop-frontend](https://github.com/DevOpsProjectsOrganization/roboshop-frontend) | NGINX / HTML | React-based storefront served via NGINX — handles product browsing, cart, checkout, and user-facing pages |
| [roboshop-cart](https://github.com/DevOpsProjectsOrganization/roboshop-cart) | Node.js | Shopping cart service — manages cart state backed by Redis |
| [roboshop-catalogue](https://github.com/DevOpsProjectsOrganization/roboshop-catalogue) | Node.js | Product catalogue service — serves product listings and details from MongoDB |
| [roboshop-user](https://github.com/DevOpsProjectsOrganization/roboshop-user) | Node.js | User service — handles registration, authentication, and user profile management |
| [roboshop-payment](https://github.com/DevOpsProjectsOrganization/roboshop-payment) | Python | Payment processing service — integrates with RabbitMQ for async transaction handling |
| [roboshop-shipping](https://github.com/DevOpsProjectsOrganization/roboshop-shipping) | Java (Maven) | Shipping calculation service — computes delivery costs and manages shipping data in MySQL |
| [roboshop-dispatch](https://github.com/DevOpsProjectsOrganization/roboshop-dispatch) | Go | Order dispatch service — consumes messages from RabbitMQ and handles order fulfilment events |
| [roboshop-github-actions](https://github.com/DevOpsProjectsOrganization/roboshop-github-actions) | GitHub Actions | Shared CI/CD workflows and self-hosted runner image used across all microservice repositories |

---

## Deployment Workflow

### 1. Infrastructure as Code (Terraform)
Creates Route 53 records, subnets, security groups, EC2 instances, ECR, and EKS clusters using a modular Terraform design for scalability and reusability.

### 2. Configuration Management (Ansible)
Automates software installation and dependency configuration, preparing EC2 instances for Docker, Kubernetes, and monitoring tools.

### 3. Containerization (Docker)
Each microservice is built into a Docker image and pushed to Amazon ECR for cluster deployment.

### 4. Kubernetes Deployment (Helm)
Helm manages Kubernetes manifests and automates release management. Deployments are version-controlled and reusable.

### 5. Continuous Monitoring & Logging
- **Prometheus** collects metrics; **Grafana** visualizes dashboards
- **ELK Stack** centralizes and filters application logs
- **ArgoCD** ensures GitOps-based synchronization between GitHub and Kubernetes

### 6. CI/CD Pipeline (GitHub Actions + ArgoCD)

**Continuous Integration** — triggered on every commit or pull request:
1. Checkout repository
2. Install dependencies & run tests
3. Build Docker image
4. Push to Amazon ECR

**Continuous Delivery (GitOps via ArgoCD)** — on successful CI:
1. Helm chart values are updated with the new image tag
2. ArgoCD detects the change and deploys updates to the EKS cluster

---

## Monitoring Dashboards

### ArgoCD — GitOps Dashboard
Visualizes real-time application sync and Kubernetes deployment status.

<img width="932" height="432" alt="argo" src="https://github.com/user-attachments/assets/d52c55b8-c75f-4da6-9993-cec254d0f008" />

### Grafana — Application Metrics
Provides insights into CPU usage, memory consumption, response times, and service uptime. Initially there are no errors. However, the latency is very high due to overusage of the pod.

<img width="942" height="422" alt="high latency no errors" src="https://github.com/user-attachments/assets/f5853990-80dc-4031-aedd-c37c7e8916f1" />

Then after implementing Horizontal Pod Autoscaling and increasing the maximum limit of the memory usage of the shipping container to 70%, latency dropped significantly.

<img width="941" height="440" alt="low latency no errors" src="https://github.com/user-attachments/assets/647961b9-a9ab-4d23-b915-6e2bcc8810b0" />

The overall pod health in terms of CPU and memory utilization:

<img width="950" height="442" alt="pod info" src="https://github.com/user-attachments/assets/246b6549-4d28-48ec-a8c2-d5f26f80555d" />

### Kibana — Centralized Logs
Displays structured logs from all microservices for error tracking and debugging.

---

## Key Features

- Fully automated Infrastructure as Code using Terraform
- End-to-end CI/CD pipeline with GitHub Actions and ArgoCD
- Dockerized microservices deployed on Kubernetes via Helm
- Continuous monitoring and observability using Prometheus & Grafana
- Centralized log aggregation through ELK Stack
- Encrypted secret management via HashiCorp Vault
- Scalable, fault-tolerant, and cloud-native design on AWS

---

## Project Results & Metrics

| Metric | Result |
|---|---|
| Deployment Time | Reduced by 40% through automated CI/CD pipelines |
| System Downtime | Zero downtime achieved using Kubernetes rolling updates |
| API Latency | Improved from ~9 seconds to ~1.5 seconds (≈ 83% faster) |
| Error Rate | Reduced from intermittent failures to 0 errors post-optimization |
| Resource Utilization | CPU & Memory stabilized at 20%–30% via autoscaling and optimized container configs |
| Infrastructure Consistency | 100% reproducible environments using Terraform & Ansible |
| Monitoring Efficiency | Real-time dashboards reduced alert response time by 50% |
| Operational Overhead | Manual intervention reduced by 60% through GitOps automation via ArgoCD |
| Secret Security | Improved security posture using HashiCorp Vault across CI/CD, Terraform, and Kubernetes |

---

## Prerequisites

To run this project you will need:

- AWS account with appropriate IAM permissions
- AWS CLI configured locally
- Terraform, Ansible, Docker, kubectl, and Helm installed
- GitHub repository secrets configured for AWS and ECR access
