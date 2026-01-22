# CI-CD-Pipeline-using-Jenkins-Docker-and-Kubernetes

## 📌 Project Overview

This project demonstrates the implementation of a CI/CD (Continuous Integration and Continuous Deployment) pipeline using Jenkins, Docker, and Kubernetes to deploy a multi-tier web application in an automated and repeatable manner.

The pipeline automates build, containerization, image publishing, and Kubernetes deployment for both tiers, following real-world DevOps practices used in production environments.

## 🛠️ Technologies Used

* **CI/CD Tool:** Jenkins

* **Containerization:** Docker

* **Container Orchestration:** Kubernetes (Minikube)

* **Frontend:** HTML (NGINX)

* **Backend:** API Service (Python)

* **Source Control:** Git & GitHub

* **Container Registry:** Docker Hub

* **OS:** Ubuntu

## 📂 Project Structure
```
.
├── frontend/
|   ├── index.html
│   ├── Dockerfile
│   ├── deployment.yaml
│   └── service.yaml
├── backend/
|   ├── app.py
|   ├── requirements.txt
│   ├── Dockerfile
│   ├── deployment.yaml
│   └── service.yaml
├── Jenkinsfile
└── README.md
```

## 🏗️ Architecture Overview

### The CI/CD workflow consists of the following stages:

1️⃣ Developer pushes code to GitHub

2️⃣ Jenkins pulls the latest code from the repository

3️⃣ Jenkins builds a Docker image for the application

4️⃣ Docker image is pushed to Docker Hub

5️⃣ Jenkins deploys the application to Kubernetes using "kubectl"

6️⃣ Kubernetes creates Pods and exposes the application via a Service

## 📋 Prerequisites
Before deploying this project, ensure the following tools and configurations are available on your system:

1️⃣ **Jenkins**
* Jenkins is required to implement the CI/CD pipeline for automated build and deployment.
 
* Install Jenkins from: https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

* Verify installation:
```
jenkins --version
```

* Allow Jenkins Docker access
```
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

2️⃣ **Docker**
* Docker is required to build and run container images for the frontend and backend services.

* Install Docker from: https://docs.docker.com/engine/install/ubuntu/

* Verify installation:
```
docker --version
```
* Allow non-root Docker access
```
sudo usermod -aG docker $USER
newgrp docker
```
3️⃣ **Kubernetes cluster (Minikube)**
* Minikube is required to run a local Kubernetes cluster for deployment and testing.

* Install Minikube from: https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download

* Start Minikube:
```
minikube start --driver=docker
```

4️⃣ **Kubectl**
* kubectl is required to interact with the Kubernetes cluster.

* Install kubectl from: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

* Verify installation:
```
kubectl version --client
```

## ⚙️ CI/CD Pipeline Stages
1️⃣ **Source Code Checkout**

* Jenkins pulls frontend and backend source code from GitHub

2️⃣ **Docker Image Build**

* Separate Docker images are built for:

    * Frontend service
    * Backend service

3️⃣ **Docker Image Push**

* Images are pushed to Docker Hub with proper tagging

4️⃣ **Kubernetes Deployment**

* Frontend and backend deployments are applied separately

* Services enable inter-pod communication and external access

## 🌐 Accessing the Application

**Frontend Access**
```
minikube service frontend-service --url
curl (URL)
```

**Backend (Internal Service)**
```
minikube service frontend-service --url
curl (URL)
```

## 🧹 Cleanup

To remove Kubernetes resources:

```
kubectl delete -f frontend/deployment.yaml
kubectl delete -f frontend/service.yaml
kubectl delete -f backend/deployment.yaml
kubectl delete -f backend/service.yaml
```







