# 🚀 Deployment of Disney Hotstar App using CI/CD Pipeline on Kubernetes

## 📘 Project Overview
This project demonstrates the **end-to-end DevOps and DevSecOps automation** for deploying a **Disney Hotstar-style microservice application** on **AWS EKS (Elastic Kubernetes Service)** using a secure **Jenkins-based CI/CD pipeline**.

It integrates **AWS Cloud Services**, **Docker**, **Kubernetes**, **SonarQube**, **Trivy**, **Prometheus**, and **Grafana** to achieve continuous integration, continuous delivery, security scanning, and real-time monitoring.

---

## 🧩 Objective
The main goal of this project is to:
> **Automate the deployment of a microservice application using a secure and efficient CI/CD pipeline on AWS EKS.**

---

## 🛠️ Tools & Technologies Used

| Category | Tools |
|-----------|-------|
| **Cloud Platform** | AWS (EKS, EC2, IAM, S3) |
| **CI/CD Tool** | Jenkins |
| **Containerization** | Docker |
| **Orchestration** | Kubernetes (EKS) |
| **Code Quality** | SonarQube |
| **Security Scanning** | Trivy |
| **Monitoring** | Prometheus, Grafana |
| **Version Control** | Git & GitHub |

---

## 🏗️ Project Architecture

```
                ┌──────────────────────────┐
                │        Developer          │
                │   (Code pushed to GitHub) │
                └─────────────┬─────────────┘
                              │
                              ▼
                   ┌─────────────────────┐
                   │     Jenkins CI/CD   │
                   │  (Build, Test, Scan │
                   │   & Deploy stages)  │
                   └──────────┬──────────┘
                              │
               ┌──────────────┴──────────────┐
               ▼                             ▼
       ┌──────────────┐              ┌────────────────┐
       │  SonarQube   │              │     Trivy       │
       │ (Code Quality│              │ (Security Scan) │
       └──────────────┘              └────────────────┘
                              │
                              ▼
                  ┌──────────────────────────────┐
                  │   Docker Image Build & Push  │
                  │  (Docker Hub / ECR Registry) │
                  └──────────────┬───────────────┘
                                 │
                                 ▼
                     ┌──────────────────────┐
                     │   AWS EKS Cluster    │
                     │ (Deploy Kubernetes   │
                     │     Manifests)       │
                     └────────────┬─────────┘
                                  │
                     ┌───────────────────────────┐
                     │ Prometheus & Grafana Stack │
                     │  (Monitoring & Alerting)  │
                     └───────────────────────────┘
```

---

## ⚙️ Step-by-Step Setup Guide

### 1️⃣ Prerequisites
- AWS account with IAM permissions
- EC2 instance (for Jenkins & other tools)
- Docker, kubectl, awscli, eksctl installed
- Jenkins configured with required plugins:
  - Git
  - Docker
  - Kubernetes CLI
  - SonarQube Scanner
  - Trivy or Wazuh (for vulnerability scan)

---

### 2️⃣ Infrastructure Setup (AWS)
1. **Create an EKS Cluster:**
   ```bash
   eksctl create cluster --name hotstar-cluster --region ap-south-1 --node-type t3.medium --nodes 2
   ```

2. **Verify Cluster:**
   ```bash
   kubectl get nodes
   ```

3. **Connect Jenkins to EKS Cluster:**
   - Copy Kubernetes config to Jenkins server (`/var/lib/jenkins/.kube/config`).

---

### 3️⃣ Jenkins Pipeline Setup
- Create a **Declarative Pipeline** with stages:
  1. **Code Checkout** (from GitHub)
  2. **Code Quality Analysis** (SonarQube)
  3. **Security Scanning** (Trivy)
  4. **Docker Build & Push**
  5. **Deploy to Kubernetes (EKS)**
  6. **Post-Deployment Monitoring**

Example `Jenkinsfile`:
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/avutashashi291/disney-hotstar-devops.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                sh 'sonar-scanner'
            }
        }
        stage('Security Scan') {
            steps {
                sh 'trivy image your-image-name:latest'
            }
        }
        stage('Build & Push Docker Image') {
            steps {
                sh 'docker build -t yourusername/hotstar-app:latest .'
                sh 'docker push yourusername/hotstar-app:latest'
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
```

---

### 4️⃣ Kubernetes Deployment
**Apply Kubernetes manifests:**
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods
```

**Access the Application:**
Once the LoadBalancer service is created, fetch its public DNS:
```bash
kubectl get svc
```
Then open:
```
http://<LoadBalancer-DNS>
```

---

### 5️⃣ Monitoring & Visualization
1. **Install Prometheus and Grafana:**
   ```bash
   kubectl apply -f monitoring/
   ```
2. **Access Grafana Dashboard:**
   ```bash
   http://<Grafana-LoadBalancer-DNS>:3000
   ```
3. **Monitor metrics:** Pod CPU, memory, and request rates.

---

## 📊 CI/CD Workflow Summary

| Stage | Description |
|--------|--------------|
| **Checkout** | Pull latest code from GitHub |
| **SonarQube** | Analyze code quality |
| **Trivy Scan** | Check image vulnerabilities |
| **Build & Push** | Build Docker image and push to registry |
| **Deploy to EKS** | Deploy app using kubectl |
| **Monitor** | Use Prometheus & Grafana to visualize metrics |

---

## 🧠 Key Learnings
- Building an automated DevSecOps workflow from scratch
- Integrating CI/CD with Kubernetes on AWS
- Implementing security scanning in pipelines
- Using Prometheus & Grafana for real-time monitoring
- Managing application lifecycle efficiently

---

## 📁 Folder Structure
```
├── Jenkinsfile
├── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   └── secret.yaml
├── monitoring/
│   ├── prometheus.yaml
│   ├── grafana-deployment.yaml
│   └── dashboards/
├── src/
│   └── (application code)
└── README.md
```

---

## 👨‍💻 Author
**Developed by:** [avutashashi291](https://github.com/avutashashi291)  
**Role:** DevOps Engineer / AWS Cloud Enthusiast  
**Objective:** To showcase complete DevSecOps automation on AWS EKS using Jenkins CI/CD.

---

## 🌟 Acknowledgement  
**“DevSecOps Project - Disney Hotstar App Deployment on Kubernetes using Jenkins CI/CD”**  
Special thanks to the creator for educational guidance.
