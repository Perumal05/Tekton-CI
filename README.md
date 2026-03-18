# 🚀 End-to-End Tekton CI Pipeline (DevSecOps Enabled)

> ⚡ This project demonstrates a complete **DevSecOps CI pipeline using Tekton**, including SAST, SCA, container scanning, and secure image promotion to a private DockerHub registry.

---

## 📌 Overview

This project implements an **end-to-end CI pipeline on Kubernetes using Tekton**, starting from source code commit to building, scanning, and securely pushing a container image.

The pipeline is:

* 🔁 Triggered via **GitHub Webhooks**
* 🌐 Exposed locally using **ngrok**
* ⚙️ Executed on a **Kind Kubernetes cluster**

---

## 🔥 Key Features

* Tekton Pipelines & Triggers (Cloud-native CI)
* Maven build & unit testing
* SonarCloud SAST scanning
* Trivy filesystem (SCA) & container scanning
* Docker image build using Kaniko
* Secure image promotion using Skopeo
* Private DockerHub registry integration
* Webhook-based automation using ngrok

---

## 🏗️ Project Structure

```bash
.
├── README.md
├── docs/
│   ├── 01-architecture.md
│   ├── 02-prerequisites.md
│   ├── 03-tekton-installation.md
│   ├── 04-dashboard-access.md
│   ├── 05-pipeline-overview.md
│   ├── 06-tasks-explained.md
│   ├── 07-secrets-setup.md
│   ├── 08-triggers-webhook.md
│   ├── 09-execution-guide.md
│   ├── 10-troubleshooting.md
│   └── 11-results-and-screenshots.md
├── pipeline.yaml
├── pipelinerun.yaml
├── tasks/
├── triggers/
```

---

## 🏗️ Architecture

![Architecture](docs/assets/pipeline.png)

---

## 🔄 Pipeline Flow

```text
Clone Repo 
   ↓
Maven Build 
   ↓
Unit Tests 
   ↓
SonarCloud Scan (SAST)
   ↓
Trivy FS Scan (SCA)
   ↓
Generate Dockerfile
   ↓
Build Image (Kaniko → Temp Repo)
   ↓
Container Scan
   ↓
Promote Image (Temp → Final Repo)
```

---

## 📸 Execution Highlights

### 🔹 Task Logs

| Task                | Screenshot                                    |
| ------------------- | --------------------------------------------- |
| Clone Repo          | ![](docs/assets/clone-repo.png)               |
| Maven Build         | ![](docs/assets/maven-build.png)              |
| Unit Tests          | ![](docs/assets/unit-test.png)                |
| Sonar Scan          | ![](docs/assets/sonar-scan.png)               |
| Trivy FS Scan       | ![](docs/assets/trivy-fs-scan.png)            |
| Generate Dockerfile | ![](docs/assets/generate-java-dockerfile.png) |
| Build Image         | ![](docs/assets/build-image.png)              |
| Container Scan      | ![](docs/assets/container-scan.png)           |
| Push Image          | ![](docs/assets/push-image.png)               |

---

## 📊 Dashboards

### 🔹 SonarCloud

![Sonar](docs/assets/sonar-scan-dashboard.png)

### 🔹 DockerHub

![DockerHub](docs/assets/dockerhub-dashboard.png)

---

## ⚙️ Tech Stack

* Kubernetes (Kind)
* Tekton Pipelines & Triggers
* Maven (Java)
* SonarCloud (SAST)
* Trivy (SCA + Image Scan)
* Kaniko (Image Build)
* Skopeo (Image Promotion)
* DockerHub (Private Registry)
* ngrok (Webhook Exposure)

---

## 🚀 Quick Start

### 1️⃣ Install Tekton

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
kubectl apply --filename https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
kubectl apply --filename https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml
kubectl apply --filename https://infra.tekton.dev/tekton-releases/dashboard/latest/release-full.yaml
```

---

### 2️⃣ Setup Namespace

```bash
kubectl create namespace java-pipeline
```

---

### 3️⃣ Apply Pipeline

```bash
kubectl apply -f ./tasks -n java-pipeline
kubectl apply -f ./pipeline.yaml -n java-pipeline
```

---

### 4️⃣ Run Pipeline (Manual)

```bash
kubectl apply -f ./pipelinerun.yaml -n java-pipeline
kubectl get pipelineruns -n java-pipeline
```

---

### 5️⃣ Enable Webhook Trigger

```bash
kubectl apply -f ./triggers -n java-pipeline
kubectl port-forward svc/el-java-ci-listener 8080:8080 -n java-pipeline
ngrok http 8080
```

Use the ngrok URL in your GitHub webhook.

---

## 📚 Detailed Documentation

👉 Follow the step-by-step guides:

* [Architecture](docs/01-architecture.md)
* [Prerequisites](docs/02-prerequisites.md)
* [Tekton Installation](docs/03-tekton-installation.md)
* [Dashboard Access](docs/04-dashboard-access.md)
* [Pipeline Overview](docs/05-pipeline-overview.md)
* [Tasks Explained](docs/06-tasks-explained.md)
* [Secrets Setup](docs/07-secrets-setup.md)
* [Triggers & Webhook](docs/08-triggers-webhook.md)
* [Execution Guide](docs/09-execution-guide.md)
* [Troubleshooting](docs/10-troubleshooting.md)
* [Results & Screenshots](docs/11-results-and-screenshots.md)

---

## 🐞 Troubleshooting (Quick)

* Pipeline not triggering → Check ngrok & webhook
* Permission issues → Verify RBAC
* Image push failure → Check DockerHub secret
* Branch issues → Handled in clone task

---

## 💡 Highlights

* ✅ Complete **DevSecOps CI pipeline**
* 🔐 Multi-layer security (SAST + SCA + Image Scan)
* 🚀 Production-style image promotion strategy
* 🌍 Real-world webhook automation

---

## 👤 Author

**Perumal S**

---

## ⭐ Support

If you found this useful, give it a ⭐ on GitHub!
