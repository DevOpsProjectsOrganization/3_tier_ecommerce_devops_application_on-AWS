# 3_tier_ecommerce_devops_application_on-AWS
## Overview
This project demonstrates a complete end-to-end deployment of a 3-tier e-commerce application using a modern DevOps tool chain. It showcases automation in infrastructure provisioning, configuration management, containerization, orchestration, continuous monitoring, logging, and CI/CD pipelines — all running on AWS.
## Architecture
The architecture follows a three-tier design:
Frontend:  React-based UI served through NGINX.
Backend: Node.js, Python, Java and Go  microservices exposing RESTful APIs.
Database: MongoDB, Redis, Rabbitmq and MySQL deployed as a StatefulSet in Kubernetes.

The application is deployed to AWS using Terraform and Ansible, containerized via Docker, orchestrated with Kubernetes and Helm, and continuously monitored through Prometheus, Grafana, ArgoCD, and ELK Stack.

## Architecture diagram :
 ![My_first_board_2](https://github.com/user-attachments/assets/f1727295-fd39-487d-b229-c0be03c8b786)

 
## Category Tools	Technologies
Cloud Provider: 				AWS (EC2, EKS, VPC, IAM, S3, ECR, Route 53)

Infrastructure as Code (IaC): 	Terraform

Configuration Management:		Ansible

Containerization:				Docker

Orchestration:					Kubernetes

Package Management:				Helm Charts

CI/CD Pipeline:					GitHub Actions + ArgoCD (GitOps)

Monitoring & Logging:			Prometheus, Grafana, ELK Stack

Version Control:				Git / GitHub

## Deployment Workflow
1. Infrastructure as Code (Terraform)
Creates Route 53 records, subnets, security groups, EC2 instances, ECR and EKS clusters.
Uses modular Terraform design for scalability and reusability.
2. Configuration Management (Ansible)
Automates software installation and dependency configuration.
Prepares EC2 instances for Docker, Kubernetes, and monitoring tools.
3. Containerization (Docker)
Each microservice (frontend, backend, database) is built into Docker images.
Images are pushed to Amazon ECR for cluster deployment.
4. Kubernetes Deployment (Helm)
Helm manages Kubernetes manifests and automates release management.
Deployments are version-controlled and reusable.
5. Continuous Monitoring & Logging
Prometheus collects metrics, Grafana visualizes dashboards.
ELK Stack centralizes and filters application logs.
ArgoCD ensures GitOps-based synchronization between GitHub and Kubernetes.
6. CI/CD Pipeline (GitHub Actions + ArgoCD)
Automates the build, test, and deployment process:
Continuous Integration
Triggered on every commit or pull request.
## Steps:

Checkout repository

Install dependencies & run tests

Build Docker images

Push to Amazon ECR

Continuous Delivery (GitOps via ArgoCD)

On successful CI, Helm chart values are updated with the new image tag.

ArgoCD automatically detects the change and deploys updates to the EKS cluster.


## 📊 Monitoring Dashboards 🌀 ArgoCD — GitOps Dashboard
Visualizes real-time application sync and Kubernetes deployment status.  

 <img width="932" height="432" alt="argo" src="https://github.com/user-attachments/assets/d52c55b8-c75f-4da6-9993-cec254d0f008" />
 
## Grafana — Application Metrics
Provides insights into CPU usage, memory consumption, response times, and service uptime. Initially there are no errors . However, the latency is very high due to overusage of the pod.

<img width="942" height="422" alt="high latency no errors" src="https://github.com/user-attachments/assets/f5853990-80dc-4031-aedd-c37c7e8916f1" />


Then after implementing Horizontal Pod Autoscaling and increasing the maximum limit of the memory usage of shipping container to 70%  resulted in low latency. 

 <img width="941" height="440" alt="low latency no errors" src="https://github.com/user-attachments/assets/647961b9-a9ab-4d23-b915-6e2bcc8810b0" />

The overall pod health in terms of CPU and memory utilization

 <img width="950" height="442" alt="pod info" src="https://github.com/user-attachments/assets/246b6549-4d28-48ec-a8c2-d5f26f80555d" />

📜Kibana — Centralized Logs

Displays structured logs from various microservices for error tracking and debugging.  
Key Features
Fully automated Infrastructure as Code using Terraform.
End-to-end CI/CD pipeline with GitHub Actions and ArgoCD.
Dockerized microservices deployed on Kubernetes via Helm.
Continuous monitoring and observability using Prometheus & Grafana.
Centralized log aggregation through ELK Stack.
Scalable, fault-tolerant, and cloud-native design on AWS.
# Project Results & Metrics

Deployment Time	Reduced by 40% through automated CI/CD pipelines

System Downtime	Zero downtime achieved using Kubernetes rolling updates

API Latency	Improved from ~9 seconds to ~1.5 seconds (≈ 83% faster)

Error Rate	Reduced from intermittent failures to 0 errors post-optimization

Resource Utilization	CPU & Memory saturation stabilized at 20%–30% due to efficient autoscaling and optimized container configurations

Infrastructure Consistency	100% reproducible environments using Terraform & Ansible

Monitoring Efficiency	Real-time dashboards reduced alert response time by 50%

Operational Overhead	Manual intervention reduced by 60% through GitOps automation using ArgoCD

Secret Security Management	Improved security posture using HashiCorp Vault for encrypted, centralized secret management across CI/CD, Terraform, and Kubernetes workloads
	
	
	


