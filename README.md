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

1️⃣ Jenkins
* Jenkins is used to implement the CI/CD pipeline for automated build and deployment.
 
* Install Jenkins from: https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

* Verify installation:

```jenkins --version```

2️⃣ Docker
```
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```
* Kubernetes cluster (Minikube)

* kubectl configured for cluster access












