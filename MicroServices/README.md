# 🔐 Zero-Trust CI/CD Pipeline for Microservices
### DevSecOps College Project | Rohit | Rahul | Praveen

![GitHub Actions](https://img.shields.io/badge/CI/CD-GitHub%20Actions-blue)
![Docker](https://img.shields.io/badge/Container-Docker-2496ED)
![Kubernetes](https://img.shields.io/badge/Orchestration-Kubernetes-326CE5)
![SonarCloud](https://img.shields.io/badge/SAST-SonarCloud-F3702A)
![Trivy](https://img.shields.io/badge/Scanner-Trivy-1904DA)
![GitLeaks](https://img.shields.io/badge/Secrets-GitLeaks-red)

---

## 📌 Project Overview

This project implements a **Zero-Trust CI/CD pipeline** for a microservices-based application using DevSecOps principles.

> **"Trust nothing, verify everything — every build is treated as potentially compromised until proven secure through automated scanning."**

The pipeline follows the **Shift-Left Security Philosophy**, meaning security checks happen at the **earliest possible stage** — before a commit, after a push, and before deployment.

---

## 🏗️ Architecture

```
Developer Machine
      |
 [git commit] ──► Pre-commit Hook (GitLeaks)
      |
 [git push]  ──► GitHub Repository
                       |
              GitHub Actions Pipeline
              |         |          |
          GitLeaks  SonarCloud  Maven Build
          (Secrets)  (SAST)     (Compile)
                                   |
                             Docker Build & Push
                                   |
                             Trivy Scan (CVEs)
                                   |
                        Kubernetes (Minikube)
                        |                  |
                 Order Service       Payment Service
                 (Port 8080)         (Port 8081)
```

---

## 👥 Team Members & Roles

| Member | Role | Responsibilities |
|--------|------|-----------------|
| **Rohit** | Microservices Developer | Order & Payment Service, REST APIs, Dockerfiles, GitHub Setup |
| **Rahul** | CI/CD Engineer | GitHub Actions Pipeline, Docker Build & Push, Automation |
| **Praveen** | Security & DevOps | SonarQube, GitLeaks, Trivy, Kubernetes Deployment |

---

## 📁 Project Structure

```
devsecops-project/
│
├── order-service/                  # Order Microservice
│     ├── src/main/java/
│     │     ├── OrderServiceApplication.java
│     │     ├── controller/
│     │     │     └── OrderController.java
│     │     ├── service/
│     │     │     └── OrderService.java
│     │     └── model/
│     │           ├── Order.java
│     │           ├── PaymentRequest.java
│     │           └── PaymentResponse.java
│     ├── src/main/resources/
│     │     └── application.properties
│     ├── Dockerfile
│     └── pom.xml
│
├── payment-service/                # Payment Microservice
│     ├── src/main/java/
│     │     ├── PaymentServiceApplication.java
│     │     ├── controller/
│     │     │     └── PaymentController.java
│     │     ├── service/
│     │     │     └── PaymentService.java
│     │     └── model/
│     │           ├── Payment.java
│     │           └── PaymentRequest.java
│     ├── src/main/resources/
│     │     └── application.properties
│     ├── Dockerfile
│     └── pom.xml
│
├── k8s/                            # Kubernetes Manifests
│     ├── order-deployment.yaml
│     ├── order-service.yaml
│     ├── payment-deployment.yaml
│     └── payment-service.yaml
│
├── .github/
│     └── workflows/
│           └── pipeline.yml        # GitHub Actions CI/CD Pipeline
│
├── .pre-commit-config.yaml         # GitLeaks Pre-commit Hook
├── docker-compose.yml              # Local Development
└── README.md
```

---

## 🛠️ Tech Stack

| Category | Tool |
|----------|------|
| Microservices | Spring Boot (Java 17) |
| Version Control | Git + GitHub |
| Containerization | Docker (Multi-stage builds) |
| CI/CD | GitHub Actions |
| Secret Detection | GitLeaks |
| SAST Scanning | SonarCloud |
| Container Scanning | Trivy |
| Orchestration | Kubernetes (Minikube) |

---

## 🔒 Shift-Left Security Layers

```
LAYER 1 — Before Commit (Local)
  Tool: GitLeaks Pre-commit Hook
  Catches: API keys, passwords, tokens

LAYER 2 — After Push (Pipeline)
  Tool: GitLeaks + SonarCloud
  Catches: Secrets in history + Code vulnerabilities

LAYER 3 — After Docker Build
  Tool: Trivy
  Catches: CVEs in OS packages + base image
```

---

## 🚀 Getting Started

### Prerequisites
- Java 17
- Docker Desktop
- Git
- Maven

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/devsecops-project.git
cd devsecops-project
```

### 2. Install Pre-commit Hook (GitLeaks)
```bash
pip install pre-commit
pre-commit install
```

### 3. Run Locally with Docker Compose
```bash
docker-compose up --build
```

### 4. Test the APIs
```bash
# Create an order
curl -X POST http://localhost:8080/orders \
  -H "Content-Type: application/json" \
  -d '{"product": "Laptop", "amount": 999}'

# Get all orders
curl http://localhost:8080/orders

# Get all payments
curl http://localhost:8081/payments
```

---

## 🔄 CI/CD Pipeline Stages

| Stage | Tool | Purpose |
|-------|------|---------|
| 1. Secret Detection | GitLeaks | Scan for hardcoded secrets |
| 2. SAST Scan | SonarCloud | Static code analysis |
| 3. Build | Maven | Compile both services |
| 4. Docker Build & Push | Docker | Create and push images |
| 5. Container Scan | Trivy | Scan images for CVEs |
| 6. Deploy | Kubernetes | Deploy to Minikube |

> Pipeline triggers automatically on every push to `main` branch.

---

## 🌐 API Endpoints

### Order Service (Port 8080)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/orders` | Create a new order |
| GET | `/orders` | Get all orders |
| GET | `/actuator/health` | Health check |

**Request Body (POST /orders):**
```json
{
  "product": "Laptop",
  "amount": 999
}
```

**Response:**
```json
{
  "orderId": "550e8400-e29b-41d4-a716-446655440000",
  "product": "Laptop",
  "amount": 999,
  "status": "CONFIRMED"
}
```

### Payment Service (Port 8081)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/payments` | Process a payment |
| GET | `/payments` | Get all payments |
| GET | `/actuator/health` | Health check |

> **Note:** Payment service has an **80% success rate** to simulate real-world scenarios.

---

## ☸️ Kubernetes Deployment

```bash
# Start Minikube
minikube start

# Deploy Payment Service first
kubectl apply -f k8s/payment-deployment.yaml
kubectl apply -f k8s/payment-service.yaml

# Deploy Order Service
kubectl apply -f k8s/order-deployment.yaml
kubectl apply -f k8s/order-service.yaml

# Check pods
kubectl get pods

# Get access URL
minikube service order-service --url
```

---

## 🧪 Testing GitLeaks (Secret Detection Demo)

```bash
# Step 1: Add fake secret to docker-compose.yml

# Step 2: Try to commit
git add docker-compose.yml
git commit -m "test"
# Result: COMMIT BLOCKED by GitLeaks ❌

# Step 3: Remove secret and commit again
# Result: Commit ALLOWED ✅
```

---

## 📊 Project Status

| Component | Status |
|-----------|--------|
| Order Service | ✅ Complete |
| Payment Service | ✅ Complete |
| Dockerfiles | ✅ Complete |
| Docker Compose | ✅ Complete |
| GitHub Actions Pipeline | ✅ Complete |
| GitLeaks | ✅ Complete |
| SonarCloud | ✅ Complete |
| Trivy | ✅ Complete |
| Kubernetes Deployment | ✅ Complete |

---

## 📄 License

This project is developed for academic purposes as part of B.Tech Computer Science & Engineering curriculum.

---

*Made with ❤️ by Rohit, Rahul & Praveen*