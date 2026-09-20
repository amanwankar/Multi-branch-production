# Multi-Branch Production

A production-style DevOps implementation of a Flask-based e-commerce application using modern CI/CD, containerization, security scanning, Kubernetes, Helm, GitOps, and monitoring tools.

The project demonstrates how application code moves from **GitHub source code → Docker image → Jenkins CI/CD → Security Scan → Kubernetes → Helm → Argo CD → Prometheus → Grafana**.

---

## 📌 Project Overview

**Multi-Branch Production** is a Flask-based ShopEasy e-commerce application deployed through a complete DevOps workflow.

The main objective of this project is to demonstrate practical implementation of:

* Git & GitHub
* Git branching and pull requests
* Python Flask
* Docker
* Docker Hub
* Jenkins CI/CD
* Trivy security scanning
* Kubernetes
* Minikube
* Helm
* Argo CD / GitOps
* Prometheus
* Grafana
* Application metrics
* Kubernetes monitoring

---

# 🏗️ Architecture

```text
                    ┌──────────────────┐
                    │     Developer    │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │     GitHub       │
                    │  Branches / PR   │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │     Jenkins      │
                    │      CI/CD       │
                    └────────┬─────────┘
                             │
              ┌──────────────┴──────────────┐
              ▼                             ▼
       ┌─────────────┐              ┌─────────────┐
       │ Docker Build│              │    Trivy    │
       └──────┬──────┘              │Security Scan│
              │                     └─────────────┘
              ▼
       ┌─────────────┐
       │ Docker Hub  │
       └──────┬──────┘
              │
              ▼
       ┌──────────────────┐
       │     Argo CD      │
       │      GitOps      │
       └────────┬─────────┘
                │
                ▼
       ┌──────────────────┐
       │    Kubernetes    │
       │     Minikube     │
       └────────┬─────────┘
                │
        ┌───────┴────────┐
        ▼                ▼
   ┌─────────┐      ┌─────────┐
   │ Flask   │      │ Service │
   │  Pods    │      │         │
   └────┬────┘      └─────────┘
        │
        │ /metrics
        ▼
   ┌─────────────┐
   │ Prometheus  │
   └──────┬──────┘
          │
          ▼
   ┌─────────────┐
   │   Grafana   │
   └─────────────┘
```

---

# 🧰 Technology Stack

| Category           | Technology     |
| ------------------ | -------------- |
| Application        | Python / Flask |
| Source Control     | Git            |
| Repository         | GitHub         |
| Containerization   | Docker         |
| Image Registry     | Docker Hub     |
| CI/CD              | Jenkins        |
| Security           | Trivy          |
| Orchestration      | Kubernetes     |
| Local Kubernetes   | Minikube       |
| Package Management | Helm           |
| GitOps             | Argo CD        |
| Metrics            | Prometheus     |
| Visualization      | Grafana        |

---

# 📁 Project Structure

```text
Multi-branch-production/
│
├── app.py
├── requirements.txt
├── Dockerfile
├── Jenkinsfile
│
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
├── helm/
│   └── multibranch-flask/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── _helpers.tpl
│           ├── deployment.yaml
│           ├── service.yaml
│           └── servicemonitor.yaml
│
└── screenshots/
```

---

# 1️⃣ Application Development

The application is built using Flask.

The Flask application runs on:

```text
Port: 5000
```

The application provides e-commerce functionality including:

* Product listing
* Shopping cart
* Add to cart
* Remove from cart
* Quantity updates
* Checkout
* HTTP API endpoints

The application also exposes Prometheus metrics through:

```text
/metrics
```

### Run the application

```bash
python3 app.py
```

### Test the application

```bash
curl http://localhost:5000
```

### Test application metrics

```bash
curl http://localhost:5000/metrics
```

The `/metrics` endpoint exposes Flask and Python process metrics that can later be collected by Prometheus.

---

# 2️⃣ Git and GitHub

Git is used for source-code management.

The project follows a multi-branch development approach.

Main branches:

```text
main
featureA
featureB
```

### Check repository

```bash
git status
```

### Check branches

```bash
git branch -a
```

### View commit history

```bash
git log --oneline --graph --all
```

### Check GitHub remote

```bash
git remote -v
```

### Create feature branch

```bash
git checkout -b featureA
```

```bash
git checkout -b featureB
```

### Commit changes

```bash
git add .
git commit -m "message"
```

### Push changes

```bash
git push origin main
```

### Pull latest changes

```bash
git pull origin main
```

### Development workflow

```text
Feature Branch
      ↓
Code Changes
      ↓
Commit
      ↓
GitHub
      ↓
Pull Request
      ↓
main
```

---

# 3️⃣ Python Virtual Environment

A Python virtual environment was used to isolate project dependencies.

### Create environment

```bash
python3 -m venv .venv
```

### Activate environment

```bash
source .venv/bin/activate
```

### Install dependencies

```bash
pip install -r requirements.txt
```

### Verify packages

```bash
pip list
```

---

# 4️⃣ Docker Containerization

Docker is used to package the Flask application together with its runtime dependencies.

The Docker image is based on:

```text
python:3.11-slim
```

The container exposes:

```text
5000
```

### Check Docker

```bash
docker --version
```

### Build Docker image

```bash
docker build -t multibranch-flask-app:latest .
```

### Check images

```bash
docker images
```

### Run container

```bash
docker run -d \
  --name multibranch-flask-app \
  -p 5000:5000 \
  multibranch-flask-app:latest
```

### Check running containers

```bash
docker ps
```

### View application logs

```bash
docker logs multibranch-flask-app
```

### Test containerized application

```bash
curl http://localhost:5000
```

### Test container metrics

```bash
curl http://localhost:5000/metrics
```

---

# 5️⃣ Docker Hub

Docker Hub is used as the container image registry.

Image repository:

```text
amanwankar18/multibranch-flask-app
```

### Docker login

```bash
docker login
```

### Tag image

```bash
docker tag multibranch-flask-app:latest \
  amanwankar18/multibranch-flask-app:latest
```

### Push image

```bash
docker push amanwankar18/multibranch-flask-app:latest
```

### Verify local image

```bash
docker images
```

The image can then be consumed by Kubernetes.

---

# 6️⃣ Jenkins CI/CD

Jenkins automates the application delivery pipeline.

The Jenkins pipeline performs:

```text
Checkout
   ↓
Test
   ↓
Docker Build
   ↓
Trivy Security Scan
   ↓
Docker Push
```

### Jenkins environment verification

```bash
sudo systemctl status jenkins
```

```bash
java -version
```

```bash
docker --version
```

The Jenkins pipeline checks out the `main` branch from GitHub.

### Application test stage

```bash
python3 -m py_compile app.py
```

This verifies that the Python source can be compiled successfully.

### Docker build stage

```bash
docker build \
  -t ${IMAGE_NAME}:${BUILD_NUMBER} \
  -t ${IMAGE_NAME}:latest \
  .
```

### Docker push stage

```bash
docker push ${IMAGE_NAME}:${BUILD_NUMBER}
```

```bash
docker push ${IMAGE_NAME}:latest
```

### CI/CD flow

```text
GitHub
   ↓
Jenkins
   ↓
Checkout
   ↓
Python Test
   ↓
Docker Build
   ↓
Trivy Scan
   ↓
Docker Push
```

---

# 7️⃣ Trivy Security Scanning

Trivy is used to scan Docker images for known security vulnerabilities.

### Check Trivy

```bash
trivy --version
```

### Scan Docker image

```bash
trivy image \
  --severity HIGH,CRITICAL \
  amanwankar18/multibranch-flask-app:latest
```

The Jenkins pipeline also performs the scan before pushing the image.

```bash
trivy image \
  --severity HIGH,CRITICAL \
  ${IMAGE_NAME}:${BUILD_NUMBER}
```

### Security workflow

```text
Docker Image
     ↓
   Trivy
     ↓
HIGH / CRITICAL Vulnerability Report
```

---

# 8️⃣ Kubernetes

Kubernetes is used to orchestrate the application containers.

### Check kubectl

```bash
kubectl version --client
```

### Check cluster nodes

```bash
kubectl get nodes
```

### Check namespaces

```bash
kubectl get namespaces
```

### Check pods

```bash
kubectl get pods
```

### Check deployments

```bash
kubectl get deployments
```

### Check services

```bash
kubectl get services
```

### Application resources

```bash
kubectl get pods -n default
```

```bash
kubectl get deployment multibranch-flask
```

```bash
kubectl get svc multibranch-flask
```

### Deployment details

```bash
kubectl describe deployment multibranch-flask
```

### Application logs

```bash
kubectl logs deployment/multibranch-flask
```

---

# 9️⃣ Minikube

Minikube provides the local Kubernetes cluster used for development and testing.

### Check Minikube

```bash
minikube version
```

### Start Minikube

```bash
minikube start --driver=docker
```

### Check cluster status

```bash
minikube status
```

Expected healthy components:

```text
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

### Check Minikube IP

```bash
minikube ip
```

### Access Kubernetes service

```bash
minikube service multibranch-flask --url
```

### Minikube architecture

```text
Minikube
   ↓
Kubernetes
   ↓
Deployment
   ↓
Pods
   ↓
Service
   ↓
Flask Application
```

---

# 🔟 Helm

Helm is used to package and manage the Kubernetes application.

Chart:

```text
helm/multibranch-flask/
```

### Helm version

```bash
helm version
```

### List Helm releases

```bash
helm list
```

### List releases across namespaces

```bash
helm list -A
```

### Validate Helm chart

```bash
helm lint helm/multibranch-flask
```

### Render Kubernetes manifests

```bash
helm template \
  multibranch-flask \
  helm/multibranch-flask
```

### Install Helm release

```bash
helm install \
  multibranch-flask \
  helm/multibranch-flask
```

### Upgrade release

```bash
helm upgrade \
  multibranch-flask \
  helm/multibranch-flask
```

### Check release status

```bash
helm status multibranch-flask
```

### Helm structure

```text
multibranch-flask/
│
├── Chart.yaml
├── values.yaml
│
└── templates/
    ├── _helpers.tpl
    ├── deployment.yaml
    ├── service.yaml
    └── servicemonitor.yaml
```

---

# 1️⃣1️⃣ Argo CD / GitOps

Argo CD implements GitOps deployment.

Argo CD continuously uses the Kubernetes configuration stored in GitHub.

### Check Argo CD components

```bash
kubectl get pods -n argocd
```

### Check applications

```bash
kubectl get applications -n argocd
```

### Check application

```bash
kubectl get application \
  multibranch-flask \
  -n argocd
```

### Check Sync
