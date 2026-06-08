# CI/CD Pipeline Automation with Jenkins, Docker, and Kubernetes

## Overview

This project demonstrates the implementation of a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline using GitHub, Jenkins, Docker, and Kubernetes.

The pipeline automatically detects code changes in a GitHub repository using Jenkins Poll SCM, builds a Docker image, pushes it to Docker Hub, and deploys the latest version to a Kubernetes cluster.

This solution reduces manual intervention, improves deployment consistency, and follows modern DevOps practices for application delivery.

---

## Architecture

```text
Developer
    │
    ▼
GitHub Repository
    │
    │ Poll SCM
    ▼
Jenkins Pipeline
    │
    ├── Source Code Checkout
    ├── Docker Image Build
    ├── Docker Image Push
    └── Kubernetes Deployment
    │
    ▼
Docker Hub
    │
    ▼
Kubernetes Cluster
    │
    ▼
Application Service
```

---

## Technology Stack

| Component  | Purpose                    |
| ---------- | -------------------------- |
| GitHub     | Source Code Management     |
| Jenkins    | CI/CD Automation           |
| Docker     | Containerization           |
| Docker Hub | Container Registry         |
| Kubernetes | Container Orchestration    |
| Groovy     | Jenkins Pipeline Scripting |
| Nginx      | Web Server                 |

---

## Project Structure

```text
finacplus-assessment/
│
├── Dockerfile
├── Jenkinsfile
├── deployment.yaml
├── index.html
└── README.md
```

---

## CI/CD Workflow

### Continuous Integration

1. Developer pushes code to GitHub.
2. Jenkins Poll SCM detects changes.
3. Jenkins automatically triggers the pipeline.
4. Source code is checked out from GitHub.
5. Docker image is built.

### Continuous Deployment

1. Docker image is pushed to Docker Hub.
2. Kubernetes deployment manifest is updated.
3. Jenkins deploys the latest image to Kubernetes.
4. Application becomes available through a Kubernetes Service.

---

## Pipeline Stages

### Stage 1: Source Code Checkout

Retrieves the latest source code from the GitHub repository.

```bash
git clone <repository-url>
```

### Stage 2: Docker Image Build

Builds the Docker image from the Dockerfile.

```bash
docker build -t adityakul548/sample-app:${BUILD_NUMBER} .
```

### Stage 3: Docker Image Push

Authenticates with Docker Hub and pushes the image.

```bash
docker push adityakul548/sample-app:${BUILD_NUMBER}
```

### Stage 4: Kubernetes Deployment

Updates the image tag and deploys the application.

```bash
kubectl apply -f deployment.yaml
```

---

## Jenkins Poll SCM Configuration

The project uses Poll SCM to automatically detect repository changes.

```text
H/2 * * * *
```

This configuration checks the repository every two minutes for new commits.

---

## Docker Image

Docker image repository:

```text
adityakul548/sample-app
```

Example:

```bash
docker pull adityakul548/sample-app:latest
```

---

## Kubernetes Deployment

### Deployment

Responsible for managing application pods and ensuring desired state.

```yaml
kind: Deployment
replicas: 2
```

### Service

Exposes the application through a Kubernetes NodePort service.

```yaml
kind: Service
type: NodePort
```

---

## Prerequisites

The following components must be installed before running this project:

* Java 17+
* Jenkins
* Git
* Docker
* Kubernetes Cluster (Minikube or Kubernetes)
* kubectl

---

## Installation

### Clone Repository

```bash
git clone https://github.com/Aditya422-ai/finacplus-assesment.git
cd finacplus-assesment
```

### Build Docker Image

```bash
docker build -t sample-app .
```

### Run Locally

```bash
docker run -d -p 8080:80 sample-app
```

### Deploy to Kubernetes

```bash
kubectl apply -f deployment.yaml
```

---

## Verification

Check deployments:

```bash
kubectl get deployments
```

Check pods:

```bash
kubectl get pods
```

Check services:

```bash
kubectl get svc
```

---

## Security Considerations

* Credentials are stored securely in Jenkins Credentials Manager.
* Docker Hub authentication uses credential binding.
* Kubernetes access is restricted through kubeconfig permissions.
* Sensitive information is not hardcoded in source code.
* Pipeline execution follows least-privilege principles.

---

## Monitoring and Logging Recommendations

### Jenkins

* Build History
* Console Logs
* Pipeline Stage View

### Kubernetes

```bash
kubectl logs <pod-name>
```

```bash
kubectl describe pod <pod-name>
```
---

## Results

The implemented solution successfully:

* Automates build and deployment processes.
* Reduces manual deployment effort.
* Ensures consistent application delivery.
* Supports scalable Kubernetes deployments.
* Demonstrates industry-standard DevOps practices.

---

## Author

**Aditya Kulkarni**

DevOps Engineer

Technologies: Jenkins, Docker, Kubernetes, Linux, Git, CI/CD, AWS, Python
