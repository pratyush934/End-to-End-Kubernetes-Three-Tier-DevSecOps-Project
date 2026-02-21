# End-to-End Kubernetes Three-Tier DevSecOps Project



## Overview

A production-ready **Three-Tier Web Application** deployed on **AWS EKS** with a complete DevSecOps pipeline. This project demonstrates end-to-end implementation of modern cloud-native practices including CI/CD, GitOps, security scanning, and monitoring.

### Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | React.js (Node 20) |
| **Backend** | Node.js (Node 20) + Express |
| **Database** | MongoDB |
| **Container Runtime** | Docker |
| **Orchestration** | Kubernetes (AWS EKS) |
| **CI/CD** | Jenkins |
| **GitOps** | ArgoCD |
| **IaC** | Terraform |
| **Registry** | AWS ECR |
| **Monitoring** | Prometheus + Grafana |
| **Security** | SonarQube, Trivy |

## Project Structure

\`\`\`
├── Application-Code/
│   ├── frontend/          # React.js frontend application
│   └── backend/           # Node.js backend API
├── Jenkins-Pipeline-Code/
│   ├── Jenkinsfile-Frontend   # CI/CD pipeline for frontend
│   └── Jenkinsfile-Backend    # CI/CD pipeline for backend
├── Jenkins-Server-TF/     # Terraform configs for Jenkins server
└── Kubernetes-Manifests-file/
    ├── Frontend/          # Frontend deployment & service
    ├── Backend/           # Backend deployment & service
    ├── Database/          # MongoDB with persistent storage
    └── ingress.yaml       # Ingress configuration
\`\`\`

## Architecture

\`\`\`
                    ┌─────────────┐
                    │   Ingress   │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
    ┌─────▼─────┐    ┌─────▼─────┐    ┌─────▼─────┐
    │ Frontend  │    │  Backend  │    │  MongoDB  │
    │  (React)  │───▶│ (Node.js) │───▶│    (DB)   │
    └───────────┘    └───────────┘    └───────────┘
\`\`\`

## Features

- **CI/CD Pipeline**: Automated build, test, and deployment using Jenkins
- **Security Scanning**: 
  - SonarQube for code quality analysis
  - Trivy for container image vulnerability scanning
- **GitOps**: ArgoCD for declarative, Git-based deployments
- **Infrastructure as Code**: Terraform for AWS resource provisioning
- **Containerization**: Multi-stage Docker builds with Node 20
- **Monitoring**: Prometheus metrics with Grafana dashboards
- **Persistent Storage**: MongoDB with PV/PVC for data persistence
- **AWS Region**: Deployed in \`ap-south-1\` (Mumbai)

## Prerequisites

- AWS Account with appropriate permissions
- AWS CLI configured
- Terraform installed
- kubectl configured
- Docker installed
- Jenkins server (can be provisioned using provided Terraform)

## Quick Start

### 1. Provision Jenkins Server

\`\`\`bash
cd Jenkins-Server-TF
terraform init
terraform apply -var-file=variables.tfvars
\`\`\`

### 2. Configure Jenkins

Set up the following credentials in Jenkins:
- \`ACCOUNT_ID\` - AWS Account ID
- \`ECR_REPO1\` - Frontend ECR repository name
- \`ECR_REPO2\` - Backend ECR repository name
- \`github-cred\` - GitHub credentials (username/password or PAT)

### 3. Create EKS Cluster

\`\`\`bash
eksctl create cluster --name three-tier-cluster --region ap-south-1
\`\`\`

### 4. Deploy Application

\`\`\`bash
kubectl apply -f Kubernetes-Manifests-file/Database/
kubectl apply -f Kubernetes-Manifests-file/Backend/
kubectl apply -f Kubernetes-Manifests-file/Frontend/
kubectl apply -f Kubernetes-Manifests-file/ingress.yaml
\`\`\`

### 5. Set up ArgoCD (GitOps)

\`\`\`bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
\`\`\`

## CI/CD Pipeline Stages

| Stage | Description |
|-------|-------------|
| Clean Workspace | Remove previous build artifacts |
| Checkout | Clone repository from GitHub |
| SonarQube Analysis | Static code analysis |
| Quality Gate | Enforce code quality standards |
| Trivy FS Scan | Scan filesystem for vulnerabilities |
| Docker Build | Build container image |
| ECR Push | Push image to AWS ECR |
| Trivy Image Scan | Scan container image |
| Update Manifest | Update Kubernetes deployment with new image tag |

## Namespace

All Kubernetes resources are deployed in the \`three-tier\` namespace:

\`\`\`bash
kubectl create namespace three-tier
\`\`\`


---
