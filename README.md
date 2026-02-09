📌 Project Overview

This project demonstrates a hands-on DevOps implementation for a Spring Boot application, covering:

CI/CD pipeline automation

Dockerized application builds

Kubernetes deployments with multi-environment support (Dev & QA)

MySQL and Kafka integration

Code quality enforcement using SonarQube Quality Gates

Stability, rollback, and cost-optimization best practices

The goal is to simulate a real-world DevOps workflow using industry-standard tools.

🛠️ Tech Stack

Language: Java 17

Framework: Spring Boot

Build Tool: Maven

CI/CD: GitHub Actions

Code Quality: SonarQube + Quality Gate

Containerization: Docker

Orchestration: Kubernetes (k8s)

Database: MySQL

Messaging: Apache Kafka

OS: Ubuntu (local / cloud compatible)


⚙️ CI/CD Pipeline (GitHub Actions)

The CI/CD pipeline is fully automated and follows a fail-fast approach.

Pipeline Stages

Checkout Source Code

Build Application (Maven)

Run Unit Tests

SonarQube Code Analysis

Quality Gate Validation

Build Docker Image

Push Image to Registry

Deploy to Kubernetes (Dev Namespace)

📌 Quality Gate Enforcement
The pipeline fails immediately if SonarQube quality gate conditions are not met (code smells, bugs, coverage, vulnerabilities).

🔍 SonarQube & Quality Gate

SonarQube is integrated into the pipeline to ensure:

Code quality

Security best practices

Maintainability

Technical debt control

Key Checks

Bugs & vulnerabilities

Code smells

Test coverage

Duplications

Deployment proceeds only if the Quality Gate passes.

🐳 Docker Strategy

Multi-stage Docker build for optimized image size

Maven used only during build stage

Lightweight JRE image for runtime

Non-root container execution

Container-aware JVM memory settings

This approach improves security, performance, and cost efficiency.

☸️ Kubernetes Deployment
Environments

Dev Namespace

QA Namespace

Each environment is isolated using Kubernetes namespaces.

Kubernetes Resources

Namespace

Deployment

Service

ConfigMaps

Resource requests & limits

Liveness & Readiness probes

☸️This project uses Argo CD for continuous delivery following GitOps principles.

 How It Works

GitHub is the single source of truth

Kubernetes manifests are stored in this repository

Argo CD continuously monitors the repo

Any change to Kubernetes YAMLs is automatically synced to the cluster

📌 This ensures:

Environment isolation

Better resource utilization

High availability and reliability

GitOps Deployment with Argo CD

✅ Prerequisites
git
docker
kubectl
maven

1️⃣ Clone the Repository
git clone https://github.com/Sjon1111/Java-devops-project.git
cd Java-devops-project
2️⃣ Build and Test Locally
mvn clean package
3️⃣ Build Docker Image
docker build -t java-devops-app:latest .
4️⃣ Start Kubernetes Cluster (Example: Minikube)
minikube start
kubectl get nodes
5️⃣ Create Kubernetes Namespaces
kubectl apply -f k8s/dev/namespace.yaml
kubectl apply -f k8s/qa/namespace.yaml
6️⃣ Deploy Application to Dev Environment
kubectl apply -f k8s/dev

9️⃣ Argo CD Installation

Install Argo CD in the cluster:

kubectl create namespace argocd
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


Access Argo CD:

kubectl port-forward svc/argocd-server -n argocd 8080:443


Login:

kubectl get secret argocd-initial-admin-secret -n argocd \
  -o jsonpath="{.data.password}" | base64 -d

🔁 GitOps Deployment with Argo CD

Kubernetes manifests are stored in GitHub

Argo CD continuously watches the repository

Any change to k8s/dev or k8s/qa is automatically synced

Rollback

To rollback:

Revert the Git commit

Argo CD syncs the previous state automatically

🔁 Stability, Cost Optimization & Operational Strategy
1. Ensuring Stability Across Dev / QA / Prod

Stability across environments is achieved through environment isolation, configuration management, and controlled deployments.

Namespace isolation:
Separate Kubernetes namespaces (dev, qa, prod) prevent cross-environment impact.

Environment-specific configuration:
ConfigMaps and environment variables are used instead of hardcoded values.

Consistent deployment strategy:
The same Docker image and Kubernetes manifests are promoted across environments, reducing configuration drift.

Health checks:
Liveness and readiness probes ensure only healthy pods receive traffic.

CI quality gates:
SonarQube Quality Gates ensure only high-quality code progresses to deployment.

GitOps with Argo CD:
Git acts as the single source of truth, ensuring predictable and auditable deployments.

2. Rollback Strategy for Failed Deployments

Multiple rollback mechanisms are implemented to ensure fast recovery:

Kubernetes Rollback
kubectl rollout undo deployment <deployment-name> -n dev


This restores the previous ReplicaSet.
