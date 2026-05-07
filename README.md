# Flask AKS DevOps Project

## Overview

This project demonstrates a complete cloud-native deployment workflow using:

* Python Flask
* Docker
* Azure Container Registry (ACR)
* Azure Kubernetes Service (AKS)
* Azure DevOps Pipelines
* Jenkins CI/CD
* GitHub
* Kubernetes

The objective of the project was to build, containerise, deploy, and automate a Flask application using modern DevOps practices.

---

# Architecture

```text
Developer (Mac)
    ↓
GitHub Repository
    ↓
CI/CD Pipeline
(Azure DevOps or Jenkins)
    ↓
Docker Build
    ↓
Azure Container Registry (ACR)
    ↓
Azure Kubernetes Service (AKS)
    ↓
Public LoadBalancer Service
    ↓
Flask Web Application
```

---

# Project Objectives

This project was built to:

* Learn containerisation with Docker
* Understand Kubernetes deployments and services
* Deploy applications to AKS
* Build CI/CD pipelines using Azure DevOps
* Build alternative CI/CD pipelines using Jenkins
* Practise troubleshooting real-world cloud deployment issues
* Gain hands-on DevOps and cloud engineering experience

---

# Technologies Used

| Technology               | Purpose                                       |
| ------------------------ | --------------------------------------------- |
| Python Flask             | Web application                               |
| Docker                   | Containerisation                              |
| Azure Container Registry | Image storage                                 |
| Azure Kubernetes Service | Kubernetes platform                           |
| Azure DevOps             | CI/CD automation                              |
| Jenkins                  | Alternative CI/CD platform                    |
| GitHub                   | Source control                                |
| Azure CLI                | Azure resource management                     |
| kubectl                  | Kubernetes management                         |
| YAML                     | Kubernetes manifests and pipeline definitions |

---

# Application Structure

```text
flask-aks-demo/
├── app.py
├── requirements.txt
├── Dockerfile
├── Jenkinsfile
├── azure-pipelines.yml
├── SETUP-NOTES.md
├── .gitignore
└── k8s/
    ├── deployment.yaml
    └── service.yaml
```

---

# Flask Application

The application is a simple Flask web service that returns:

```text
Hello from Flask on AKS!
```

The purpose of the lightweight application was to focus on:

* containerisation
* deployment workflows
* Kubernetes concepts
* CI/CD automation

rather than application complexity.

---

# Docker Containerisation

The application was containerised using Docker.

## Key Docker Concepts Used

* Dockerfile
* Container image creation
* Image tagging
* Multi-platform builds
* Docker registry authentication

## Important Learning Point

Because the development machine used Apple Silicon, the image had to be built for:

```text
linux/amd64
```

to ensure compatibility with AKS worker nodes.

---

# Azure Kubernetes Service (AKS)

The project uses AKS to host the containerised Flask application.

## Kubernetes Resources Used

### Deployment

Responsible for:

* managing pods
* maintaining replica state
* rolling updates
* self-healing

### Service (LoadBalancer)

Responsible for:

* exposing the application publicly
* assigning an external IP
* routing traffic to pods

---

# CI/CD with Azure DevOps

An Azure DevOps pipeline was created to automate:

1. Docker image build
2. Push image to Azure Container Registry
3. AKS authentication
4. Kubernetes deployment
5. Rolling update

## Azure DevOps Pipeline Flow

```text
GitHub Push
    ↓
Azure DevOps Pipeline Trigger
    ↓
Build Docker Image
    ↓
Push Image to ACR
    ↓
Deploy to AKS
```

---

# CI/CD with Jenkins

A Jenkins pipeline was also implemented as an alternative CI/CD solution.

Jenkins was deployed locally using Docker Desktop.

## Jenkins Pipeline Stages

* Checkout
* Azure Login
* Build and Push Image
* Deploy to AKS

## Key Jenkins Concepts Learned

* Credentials management
* Service principals
* Pipeline-as-code
* Docker socket permissions
* Jenkinsfile structure
* Build automation

---

# Key Troubleshooting Experiences

This project included several real-world troubleshooting scenarios.

## Azure Provider Registration Issues

Resolved missing provider registrations for:

* Microsoft.ContainerRegistry
* Microsoft.ContainerService

## Azure Quota Limits

Encountered VM quota limitations when provisioning AKS.

Solution:

* switched to smaller VM size
* used Standard_B2s_v2

## Kubernetes Image Pull Errors

Encountered:

```text
ErrImagePull
ImagePullBackOff
401 Unauthorized
```

Solutions:

* reattached ACR to AKS
* rebuilt Docker image for linux/amd64
* verified ACR permissions

## Jenkins Docker Permission Errors

Encountered Docker daemon permission issues inside Jenkins container.

Solution:

* added Jenkins user to Docker group
* updated Docker socket permissions

---

# Security Considerations

The project used:

* Azure service principals
* Jenkins credentials management
* ACR authentication
* Kubernetes namespace isolation

Sensitive credentials were intentionally excluded from GitHub.

---

# Lessons Learned

This project provided practical exposure to:

* cloud infrastructure deployment
* Kubernetes operations
* CI/CD automation
* Docker workflows
* Azure services
* infrastructure troubleshooting
* YAML configuration
* Git-based workflows

It also reinforced the importance of:

* debugging skills
* reading error logs carefully
* infrastructure cleanup
* cloud cost awareness

---

# Future Improvements

Potential future enhancements include:

* Terraform infrastructure provisioning
* Helm charts
* Prometheus and Grafana monitoring
* Horizontal Pod Autoscaling
* HTTPS ingress controller
* GitHub Actions integration
* Multi-environment deployments

---

# Screenshots

Recommended screenshots to include:

* AKS cluster overview
* Azure DevOps successful pipeline
* Jenkins successful pipeline
* Running Flask application
* kubectl get pods
* GitHub repository

---

# Outcome

This project successfully demonstrated:

* end-to-end container deployment
* Kubernetes orchestration
* CI/CD automation using multiple platforms
* practical cloud engineering workflows
* DevOps troubleshooting and operational skills

The application was successfully deployed and accessible publicly through AKS.

---

# Author

Built as a hands-on DevOps and cloud engineering learning project focused on practical deployment workflows, Kubernetes operations, and CI/CD automation.
