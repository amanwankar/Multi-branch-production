# Multi-Branch Production

A production-style Flask e-commerce application demonstrating a complete **DevOps and Cloud-Native workflow** using Git, GitHub, Docker, Docker Hub, Jenkins, Trivy, Kubernetes, Minikube, Helm, Argo CD, Prometheus and Grafana.

The project demonstrates how application code moves from **source control → containerization → CI/CD → security scanning → Kubernetes deployment → GitOps → monitoring**.

---

## 🚀 Project Overview

**Multi-Branch Production** is a Flask-based e-commerce application containing product browsing and shopping-cart functionality.

The main objective of this project is to implement a complete DevOps lifecycle around the application.

### Application Features

* Flask web application
* Product listing
* Shopping cart
* Add to cart
* Remove from cart
* Update quantity
* Checkout functionality
* REST-style endpoints
* Prometheus application metrics

---

# 🏗️ DevOps Architecture

```text
                         ┌─────────────────┐
                         │     GitHub      │
                         │ Source Control  │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │     Jenkins     │
                         │     CI/CD       │
                         └────────┬────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    ▼                           ▼
             Python Test                  Docker Build
                                                │
                                                ▼
                                         ┌────────────┐
                                         │   Trivy    │
                                         │   Scan     │
                                         └─────┬──────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │   Docker Hub    │
                                      │ Container Image │
                                      └────────┬────────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │    Argo CD      │
                                      │     GitOps      │
                                      └────────┬────────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │   Kubernetes    │
                                      │    Minikube     │
                                      └────────┬────────┘
                                               │
                            ┌──────────────────┴──────────────────┐
                            ▼                                     ▼
                     ┌─────────────┐                       ┌─────────────┐
                     │ Prometheus  │                       │   Grafana   │
                     │  Metrics    │ ───────────────────► │ Dashboards  │
                     └─────────────┘                       └─────────────┘
```

---

# 🛠️ Technology Stack

| Technology | Purpose                       |
| ---------- | ----------------------------- |
| Python     | Application development       |
| Flask      | Backend web framework         |
| Git        | Version control               |
| GitHub     | Source code management        |
| Docker     | Containerization              |
| Docker Hub | Container image registry      |
| Jenkins    | CI/CD automation              |
| Trivy      | Container security scanning   |
| Kubernetes | Container orchestration       |
| Minikube   | Local Kubernetes cluster      |
| Helm       | Kubernetes package management |
| Argo CD    | GitOps continuous delivery    |
| Prometheus | Metrics collection            |
| Grafana    | Monitoring and visualization  |

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
    ├── github-repository.png
    ├── github-pr.png
    ├── dockerhub.png
    ├── jenkins-pipeline.png
    ├── trivy-scan.png
    ├── kubernetes.png
    ├── minikube.png
    ├── helm.png
    ├── argocd.png
    ├── prometheus-targets.png
    └── grafana-dashboard.png
```

---

# 1️⃣ Git & GitHub

The project started with Git-based source control.

Git was used for:

* Version control
* Branching
* Feature development
* Commit history
* Pull requests
* Collaboration
* GitHub repository management

### Important Commands

```bash
git status
```

Checks the current working-tree state.

```bash
git branch -a
```

Shows local and remote branches.

```bash
git log --oneline --graph --all
```

Shows the project commit history and branch structure.

```bash
git remote -v
```

Shows the connected GitHub repository.

### Feature Branches

The project used multiple branches including:

```text
main
featureA
featureB
```

Development flow:

```text
featureA
   │
   ▼
Pull Request
   │
   ▼
main
```

---

# 2️⃣ Flask Application

The application was developed using Python Flask.

The application runs on:

```text
Port: 5000
```

### Run Application

```bash
python3 app.py
```

### Test Application

```bash
curl http://localhost:5000
```

The application also exposes Prometheus metrics.

```bash
curl http://localhost:5000/metrics
```

The `/metrics` endpoint provides application-level metrics such as:

* HTTP request count
* Request duration
* Exceptions
* Process CPU
* Process memory
* Python runtime metrics

---

# 3️⃣ Python Virtual Environment

A Python virtual environment was used to isolate project dependencies.

### Create Environment

```bash
python3 -m venv .venv
```

### Activate Environment

```bash
source .venv/bin/activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

---

# 4️⃣ Docker Containerization

The Flask application was containerized using Docker.

The Dockerfile uses:

```text
Python 3.11 Slim
```

The container exposes:

```text
5000
```

### Verify Docker

```bash
docker --version
```

### Build Image

```bash
docker build -t multibranch-flask-app:latest .
```

### View Images

```bash
docker images
```

### Run Container

```bash
docker run -d \
  --name multibranch-flask-app \
  -p 5000:5000 \
  multibranch-flask-app:latest
```

### Check Running Containers

```bash
docker ps
```

### Check Container Logs

```bash
docker logs multibranch-flask-app
```

### Test Containerized Application

```bash
curl http://localhost:5000
```

### Test Container Metrics

```bash
curl http://localhost:5000/metrics
```

---

# 5️⃣ Docker Hub

Docker Hub was used as the container image registry.

Repository:

```text
amanwankar18/multibranch-flask-app
```

### Docker Login

```bash
docker login
```

### Tag Image

```bash
docker tag multibranch-flask-app:latest \
  amanwankar18/multibranch-flask-app:latest
```

### Push Image

```bash
docker push amanwankar18/multibranch-flask-app:latest
```

### Verify Local Image

```bash
docker images
```

Docker workflow:

```text
Dockerfile
    ↓
Docker Build
    ↓
Docker Image
    ↓
Docker Hub
```

---

# 6️⃣ Jenkins CI/CD

Jenkins was implemented as the CI/CD automation server.

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

### Jenkins Environment Verification

```bash
sudo systemctl status jenkins
```

```bash
java -version
```

```bash
docker --version
```

### Application Test

Jenkins executes:

```bash
python3 -m py_compile app.py
```

This validates Python syntax before creating the Docker image.

### Docker Build

Jenkins builds the application image using:

```bash
docker build \
  -t ${IMAGE_NAME}:${BUILD_NUMBER} \
  -t ${IMAGE_NAME}:latest \
  .
```

### Docker Push

The successful pipeline pushes:

```text
amanwankar18/multibranch-flask-app:<BUILD_NUMBER>
```

and:

```text
amanwankar18/multibranch-flask-app:latest
```

---

# 7️⃣ Trivy Security Scanning

Trivy was integrated into the Jenkins pipeline to scan Docker images for security vulnerabilities.

### Verify Trivy

```bash
trivy --version
```

### Scan Docker Image

```bash
trivy image \
  --severity HIGH,CRITICAL \
  amanwankar18/multibranch-flask-app:latest
```

Jenkins performs the scan before pushing the final image.

```text
Docker Build
     ↓
Trivy Scan
     ↓
Docker Push
```

This creates a basic security gate in the CI/CD pipeline.

---

# 8️⃣ Kubernetes

Kubernetes was used for container orchestration.

The application is deployed using:

* Deployment
* Pods
* Service
* Resource requests
* Resource limits

### Verify Kubernetes

```bash
kubectl version --client
```

### Check Nodes

```bash
kubectl get nodes
```

### Check Pods

```bash
kubectl get pods
```

### Check Deployments

```bash
kubectl get deployments
```

### Check Services

```bash
kubectl get services
```

### Application Resources

```bash
kubectl get pods -n default
```

```bash
kubectl get deployment multibranch-flask
```

```bash
kubectl get svc multibranch-flask
```

### Deployment Details

```bash
kubectl describe deployment multibranch-flask
```

### Application Logs

```bash
kubectl logs deployment/multibranch-flask
```

---

# 9️⃣ Minikube

Minikube was used to create the local Kubernetes environment.

### Verify Minikube

```bash
minikube version
```

### Start Cluster

```bash
minikube start --driver=docker
```

### Check Cluster Status

```bash
minikube status
```

Expected status:

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

### Access Application Service

```bash
minikube service multibranch-flask --url
```

---

# 🔟 Helm

Helm was used to package the Kubernetes application as a reusable chart.

Chart:

```text
helm/multibranch-flask/
```

### Helm Structure

```text
multibranch-flask/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── _helpers.tpl
    ├── deployment.yaml
    ├── service.yaml
    └── servicemonitor.yaml
```

### Verify Helm

```bash
helm version
```

### View Releases

```bash
helm list
```

### Validate Chart

```bash
helm lint helm/multibranch-flask
```

### Render Kubernetes Templates

```bash
helm template \
  multibranch-flask \
  helm/multibranch-flask
```

### Install Chart

```bash
helm install \
  multibranch-flask \
  helm/multibranch-flask
```

### Upgrade Chart

```bash
helm upgrade \
  multibranch-flask \
  helm/multibranch-flask
```

### Check Release

```bash
helm status multibranch-flask
```

Helm provides the deployment abstraction:

```text
values.yaml
     ↓
Helm Templates
     ↓
Kubernetes Manifests
     ↓
Kubernetes
```

---

# 1️⃣1️⃣ Argo CD & GitOps

Argo CD was used to implement GitOps-based deployment.

Argo CD watches the GitHub repository and synchronizes the Kubernetes configuration.

GitOps flow:

```text
Developer
    ↓
GitHub
    ↓
Argo CD
    ↓
Helm
    ↓
Kubernetes
```

### Check Argo CD

```bash
kubectl get pods -n argocd
```

### Check Applications

```bash
kubectl get applications -n argocd
```

### Check Application

```bash
kubectl get application \
  multibranch-flask \
  -n argocd
```

### Check Sync & Health

```bash
kubectl get application multibranch-flask \
  -n argocd \
  -o jsonpath='{.status.sync.status}{" | "}{.status.health.status}{"\n"}'
```

Expected healthy state:

```text
Synced | Healthy
```

### Argo CD UI

Argo CD UI was exposed locally using:

```bash
kubectl port-forward \
  --address 0.0.0.0 \
  -n argocd \
  svc/argocd-server \
  8081:443
```

UI:

```text
https://localhost:8081
```

---

# 1️⃣2️⃣ Prometheus Monitoring

Prometheus was installed using the `kube-prometheus-stack`.

It is responsible for collecting Kubernetes and application metrics.

### Check Monitoring Pods

```bash
kubectl get pods -n monitoring
```

### Check Monitoring Services

```bash
kubectl get svc -n monitoring
```

### Check ServiceMonitor

```bash
kubectl get servicemonitor -n default
```

Application ServiceMonitor:

```bash
kubectl get servicemonitor \
  multibranch-flask \
  -n default
```

### Prometheus UI

Prometheus was exposed locally using:

```bash
kubectl port-forward \
  --address 0.0.0.0 \
  -n monitoring \
  svc/monitoring-kube-prometheus-prometheus \
  9090:9090
```

Prometheus:

```text
http://localhost:9090
```

The application exposes metrics through:

```text
/metrics
```

---

# 1️⃣3️⃣ Grafana

Grafana was used to visualize Prometheus metrics.

### Check Grafana

```bash
kubectl get pods -n monitoring | grep grafana
```

### Grafana UI

```bash
kubectl port-forward \
  --address 0.0.0.0 \
  -n monitoring \
  svc/monitoring-grafana \
  3001:80
```

Grafana:

```text
http://localhost:3001
```

Prometheus datasource:

```text
http://monitoring-kube-prometheus-prometheus.monitoring.svc.cluster.local:9090
```

---

# 📊 Grafana Application Metrics

The application provides Prometheus metrics through `prometheus-flask-exporter`.

### HTTP Request Rate

```promql
rate(flask_http_request_total[5m])
```

### Total Requests

```promql
flask_http_request_total
```

### Application Exceptions

```promql
rate(flask_http_request_exceptions_total[5m])
```

### Average Request Latency

```promql
rate(flask_http_request_duration_seconds_sum[5m])
/
rate(flask_http_request_duration_seconds_count[5m])
```

### 95th Percentile Latency

```promql
histogram_quantile(
  0.95,
  sum by (le) (
    rate(flask_http_request_duration_seconds_bucket[5m])
  )
)
```

### Application CPU

```promql
rate(process_cpu_seconds_total[5m])
```

### Application Memory

```promql
process_resident_memory_bytes
```

---

# 🔄 Complete CI/CD + GitOps Flow

The complete implementation follows this lifecycle:

```text
                   Developer
                       │
                       ▼
                  GitHub Repo
                       │
                       ▼
                Jenkins Pipeline
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
          Testing           Docker Build
                                 │
                                 ▼
                           Trivy Security
                               Scan
                                 │
                                 ▼
                            Docker Hub
                                 │
                                 ▼
                              Argo CD
                                 │
                                 ▼
                               Helm
                                 │
                                 ▼
                           Kubernetes
                            / Minikube
                                 │
                    ┌────────────┴────────────┐
                    ▼                         ▼
               Prometheus                 Application
                    │                         │
                    └────────────┬────────────┘
                                 ▼
                              Grafana
                              Dashboard
```

---

# 📸 Project Evidence

The following screenshots document the implementation:

| Screenshot               | Demonstrates                     |
| ------------------------ | -------------------------------- |
| `github-repository.png`  | GitHub repository                |
| `github-pr.png`          | Branching and Pull Request       |
| `dockerhub.png`          | Docker image registry            |
| `jenkins-pipeline.png`   | CI/CD pipeline                   |
| `trivy-scan.png`         | Container security scanning      |
| `kubernetes.png`         | Kubernetes resources             |
| `minikube.png`           | Local Kubernetes cluster         |
| `helm.png`               | Helm deployment                  |
| `argocd.png`             | GitOps deployment                |
| `prometheus-targets.png` | Prometheus monitoring            |
| `grafana-dashboard.png`  | Application monitoring dashboard |

---

# 📌 Important Proof Commands

For quickly demonstrating the technologies used in this project:

### Git

```bash
git branch -a
git log --oneline --graph --all
```

### Docker

```bash
docker images
docker ps
```

### Jenkins

```bash
sudo systemctl status jenkins
```

### Trivy

```bash
trivy image --severity HIGH,CRITICAL amanwankar18/multibranch-flask-app:latest
```

### Kubernetes

```bash
kubectl get nodes
kubectl get pods
kubectl get deployments
kubectl get services
```

### Minikube

```bash
minikube status
```

### Helm

```bash
helm list
helm lint helm/multibranch-flask
```

### Argo CD

```bash
kubectl get application multibranch-flask -n argocd
```

### Prometheus

```bash
kubectl get servicemonitor -n default
kubectl get pods -n monitoring
```

### Grafana

```bash
kubectl get pods -n monitoring | grep grafana
```

---

# 🎯 DevOps Skills Demonstrated

This project demonstrates practical experience with:

* Git branching and version control
* GitHub workflows
* Pull Requests
* Python Flask
* Docker containerization
* Docker Hub
* Jenkins CI/CD
* Automated testing
* Trivy container security scanning
* Kubernetes Deployments
* Kubernetes Services
* Minikube
* Helm charts
* GitOps
* Argo CD
* Prometheus
* Grafana
* Application observability
* Kubernetes monitoring
* Infrastructure automation

---

# 👨‍💻 Author

**Aman Vidhyadhar Wankar**

B.Tech Computer Science & Engineering

GitHub: `github.com/amanwankar`

LinkedIn: `linkedin.com/in/aman-w-4b1310266`

---

# ⭐ Project Summary

This project demonstrates a complete production-oriented DevOps workflow starting from source code and ending with automated deployment and observability.

```text
Code
 ↓
GitHub
 ↓
Jenkins CI/CD
 ↓
Docker
 ↓
Trivy
 ↓
Docker Hub
 ↓
Argo CD
 ↓
Helm
 ↓
Kubernetes
 ↓
Prometheus
 ↓
Grafana
```

The implementation focuses on **automation, containerization, security, GitOps, orchestration and observability**.
