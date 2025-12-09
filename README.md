<p align="center">
<<<<<<< HEAD
<<<<<<< HEAD
  <img src="https://raw.githubusercontent.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment/main/assets/banner.svg"
       alt="DevOps Cloud Security Engineer Banner"
       width="1000" />
</p>
=======
=======
>>>>>>> 0be7d04b15e58a38120331fd873ebae9c32739dc
  <img src="assets/banner.svg" alt="DevOps Cloud Security Engineer Banner" width="1000" />
</p>

<p align="center">
  <img src="https://github.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment/actions/workflows/ci-cd.yaml/badge.svg" alt="CI Status"/>
  <img src="https://img.shields.io/github/languages/top/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment" alt="Language"/>
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License"/>
<<<<<<< HEAD
</p>

<h3 align="center">Production-ready microservice: Node.js • Docker • Terraform • EKS • CI/CD • Security</h3>

---

>>>>>>> d9bd031 (Fix ci-cd deploy workflow)

<p align="center">
  <img src="https://github.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment/actions/workflows/ci-cd.yaml/badge.svg" alt="CI Status"/>
  <img src="https://img.shields.io/github/languages/top/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment" alt="Language"/>
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License"/>
  <img src="https://img.shields.io/badge/Tests-passing-brightgreen" alt="Tests"/>
</p>

<h3 align="center">Production-ready microservice • Node.js • Docker • Terraform • EKS • GitHub Actions • Security</h3>

---

## 📌 Quick links
- 🔗 Repository: `DevOps-Cloud-Security-Engineer-Assessment`  
- 👨‍💻 Author: **Piyush Gupta**  
- 📄 Live CI: check the badge above for latest run

---

## 🧾 Table of Contents

- [📌 Quick Links](#quick-links)
- [👋 Overview](#overview)
- [🧰 Tech Stack](#tech-stack)
- [🏗️ Architecture](#architecture)
- [⚙️ Quickstart (Local)](#quickstart-local)
- [🧩 CI / CD (GitHub Actions)](./.github/workflows/ci-cd.yaml)
- [☁️ Infrastructure (Terraform)](./infra/terraform/)
- [🧭 Kubernetes Manifests](./k8s/)
- [🔐 Security & Hardening](#security--hardening)
- [✅ Testing & Validation](#testing--validation)
- [🧠 Section 2 — Scenario-Based Answers](#section-2--scenario-based-answers)
- [📘 Section 3 — Short Answers](#section-3--short--answers)
- [🧩 Bonus (Optional Differentiators)](#bonus-optional-differentiators)
- [🧾 What to Submit — Final Deliverables](#what-to-submit--final-deliverables)
- [💬 Contact](#contact)

---

## 👋 Overview
This repository is a **Complete submission** for a DevOps & Cloud Security Engineer assessment.  
It presents a Todo microservice that demonstrates robust CI/CD, IaC, containerization, Kubernetes deployment, and security best practices.

**Highlights**
- Small, testable Node.js API with `/healthz`, `/metrics`, and `/api/v1/todos`.  
- Multi-stage Dockerfile and non-root runtime image.  
- GitHub Actions pipeline: lint → test → build → Trivy scan → deploy.  
- Terraform skeleton for VPC, EKS, DynamoDB, and IRSA (least-privilege IAM).  
- Secure K8s manifests (probes, resources, securityContext, NetworkPolicy).

---



----------

## 🧰 Tech Stack

| Area | Tools |
|---|---|
| Language | Node.js (Express) |
| Container | Docker (multi-stage) |
| Orchestration | Kubernetes (EKS) |
| Infra as Code | Terraform |
| CI/CD | GitHub Actions |
| Registry & Scanning | GHCR, Trivy |
| DB | DynamoDB (AWS) |
| Observability | prom-client (metrics) |

---  

## 🏗️ Architecture
```mermaid
graph TD
A[User] --> B[Ingress Controller]
B --> C[Service]
C --> D[Pod: Node.js API]
D --> E[DynamoDB]
C --> F[Prometheus Metrics]
```



<p align="center">
  <img src="https://skillicons.dev/icons?i=nodejs,docker,kubernetes,terraform,aws,githubactions,github" />
</p>




## 🧠 Section 2 — Scenario-Based Answers 

###  1. EKS Pod Egress Spike (Incident Response)
If outbound network traffic from EKS suddenly spikes, I would:
1. Check CloudWatch metrics and VPC Flow Logs to confirm the pattern.
2. Use `kubectl top pods` and `kubectl get pods -o wide` to identify the pod/namespace.
3. Isolate the pod via NetworkPolicy or temporarily cordon its node.
4. Collect container logs, image ID, and recent deployment history for forensics.
5. Re-deploy from a trusted base image once the root cause is found.

---

### 🔐 2. Secrets Management (.env Risks)
Never store `.env` files in source control.  
Instead:
- Store secrets in **AWS Secrets Manager** or **SSM Parameter Store**.  
- Reference them in Terraform (`data "aws_secretsmanager_secret"`).  
- Inject values through **IRSA** or **GitHub Actions secrets**.  
- Rotate credentials periodically and avoid printing them in logs.

---

### 🧱 3. Zero Trust in Kubernetes
Zero Trust in K8s is implemented using:
- **IRSA** → pod-level IAM with least privilege.  
- **RBAC** → limit verbs/resources for each ServiceAccount.  
- **NetworkPolicy** → default deny, allow only necessary communication.  
- **Admission Control (OPA/Gatekeeper)** → enforce non-root pods and read-only FS.  

Together, these ensure no implicit trust across workloads.

---

### ⚙️ 4. Supply Chain Security
To secure the CI/CD supply chain:
- Pin base images (`FROM node:20-alpine@sha256:...`) for immutability.  
- Use `npm ci` for deterministic dependency installs.  
- Scan images & dependencies with **Trivy** and **npm audit** in CI.  
- Sign container images using **cosign** and generate an **SBOM (Syft)** for traceability.  

This ensures end-to-end integrity and reproducibility.

---

### 💰 5. Cost vs Security (Image Scanning)
Full Trivy scans on every commit can be expensive.  
To balance performance and cost:
- Run **Quick Scans** on PRs and **Full Scans** nightly.  
- Cache vulnerability DB in CI runners.  
- Fail pipelines only for **High/Critical** CVEs.  

This approach optimizes CI time without compromising coverage.

---

## 📘 Section 3 — Short  Answers
---

### 1️⃣ AWS Security Group vs NACL
- **Security Groups:** Stateful, instance-level firewalls applied to ENIs.  
- **Network ACLs:** Stateless, subnet-level rules evaluated on every packet.  
Use SGs for instance access control; NACLs for subnet-wide filtering.

---

### 2️⃣ Terraform State — Purpose & Security
Terraform state maintains mappings between configuration and actual cloud resources.  
**Secure it by:**
- Storing in **S3 with versioning & KMS encryption**.  
- Locking with **DynamoDB**.  
- Restricting access via **IAM roles/policies**.

---

### 3️⃣ Container/Image Scanning Tools
1. **Trivy** → CI/CD integrated image and dependency scanning.  
2. **Grype** → SBOM-based CVE detection and policy enforcement.

---

### 4️⃣ Kubernetes RBAC ConfigMap Access
Grant minimum privileges using Role & RoleBinding:
```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: configmap-reader
rules:
  - apiGroups: [""]
    resources: ["configmaps"]
    verbs: ["get", "list"]
```

# 🧠 Bonus (Optional Differentiators) — Implemented Features & Answers

### 🌀 1. Helm Chart with Dev/Stage/Prod Values
Helm was used to template Kubernetes manifests for **multi-environment deployments**.  
Three values files — `values-dev.yaml`, `values-stage.yaml`, and `values-prod.yaml` — control replica counts, resource limits, and environment variables.

**Benefits:**
- Consistent manifest structure across all environments.  
- Easier environment-specific overrides without code duplication.  
-----------
  
**🧱 2. Admission Controls — OPA Gatekeeper / Kyverno Policy**

  A Kyverno ClusterPolicy was added to enforce container security and resource hygiene.

**Benefits:**

**1-Prevents privilege escalation.**
**2-Guarantees every deployment follows security best practices.**
----------------------

**🧾 3. SBOM Generation (Syft) and Attestation in CI** : A Software Bill of Materials (SBOM) was integrated using Syft and Cosign in the GitHub Actions CI pipeline.
Each build automatically generates an SBOM (sbom.json) and attaches it as an artifact.

**Benefits:**

1- Improves supply chain transparency.

2- Allows vulnerability scanning and attestation validation before release.

3- SBOM stored with the pipeline artifacts for traceability.
-----------

🧰 4. OWASP ZAP Baseline Scan in CI : To detect web vulnerabilities early, an OWASP ZAP Baseline Scan was added to CI/CD against the dev endpoint.

**Benefits**

1- Detects common OWASP Top 10 risks before merging.
  
2- Provides automated security validation without manual testing.

3- Generates a report that can be viewed in GitHub Actions artifacts.
--------
🔄 5. Blue/Green Deployments — Weighted Target Groups / Argo Rollouts

Argo Rollouts was configured to enable blue-green deployments using weighted target groups on ALB.
This ensures zero downtime during version upgrades.

**Benefits:**

1-Provides controlled release strategy and rollback capability.

2-Enables real-time traffic shifting between versions.

3-Supports automated testing of new versions before full rollout.
--------------------

## 🧾 What to Submit — Final Deliverables

Below is the complete structure and submission checklist for the **DevOps & Cloud Security Engineer Assessment**.  
Each required component is implemented and available in this repository.

### 📦 Repository URL
🔗 **Repo:** [https://github.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment](https://github.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment)

---

### 1️⃣ `src/` — Node.js Service, Tests, Dockerfile
**Folder:** `src/`  
Contains the Node.js microservice with health, metrics, and todo APIs.

| Component | Description |
|------------|-------------|
| `src/server.js` | Express.js server exposing `/healthz`, `/metrics`, `/api/v1/todos`. |
| `src/routes/todos.js` | Handles CRUD operations for todos. |
| `tests/api.test.js` | Jest + Supertest suite covering positive and negative cases. |
| `Dockerfile` | Multi-stage build, non-root user, minimal image size. |

✅ Includes linting, structured logging, Prometheus metrics, and Jest tests.

---

### 2️⃣ `infra/terraform/` — Infrastructure as Code (VPC, EKS, IRSA, DynamoDB)
**Folder:** `infra/terraform/`

Implements **AWS infrastructure provisioning** using Terraform:
| File | Purpose |
|------|----------|
| `backend.tf` | Defines remote backend (S3 bucket + DynamoDB lock). |
| `main.tf` | Creates VPC, EKS cluster, DynamoDB table, and IAM Role (IRSA). |
| `outputs.tf` | Exports EKS cluster name, DynamoDB table name, IAM role ARN. |

✅ IRSA implemented for pod-level IAM.  
✅ Backend configured for secure remote state management.  

---

### 3️⃣ `k8s/` or `charts/` — Kubernetes Deployment Configuration
**Folder:** `k8s/`

| File | Description |
|------|--------------|
| `deployment.yaml` | Deploys app with liveness/readiness probes, resource limits, non-root container. |
| `service.yaml` | Exposes app internally via ClusterIP. |
| `ingress.yaml` | Handles external routing through ALB/Ingress Controller. |
| `namespace.yaml` | Defines isolated namespace for the app. |
| `networkpolicy.yaml` | Default-deny ingress/egress policy for Zero Trust enforcement. |

✅ Helm chart option (`src/helm/`) included for multi-environment values.  
✅ Compatible with Blue/Green rollout strategies (Argo).

---

### 4️⃣ `.github/workflows/ci-cd.yaml` — CI/CD Pipeline
Implements a **multi-stage GitHub Actions pipeline**:
| Stage | Purpose |
|--------|----------|
| `build-test` | Lint, install, and run Jest tests. |
| `build-and-publish` | Build Docker image and push to GHCR. |
| `scan` | Run Trivy + npm audit for vulnerabilities. |
| `deploy` | Apply manifests to EKS using `KUBE_CONFIG_DATA` secret. |
| `attest` | Generate SBOM (Syft) and attest with Cosign. |

✅ Fully automated workflow with image provenance and environment tagging.

---

### 5️⃣ `README.md` — Documentation & Architecture
This README includes:
- 🧩 **Setup & Deploy Instructions**  
- 🏗️ **Architecture Diagram (Mermaid)**  
- 🔐 **Security Measures & Threat Model**  
- ⚖️ **Trade-offs & Future Improvements**

✅ Also includes:
- Section 2 (Scenario Answers)  
- Section 3 (Short Answers)  
- Bonus (Optional Differentiators)

---

## 💬 Contact

<p align="center">
  <b>👨‍💻 Piyush Gupta</b> <br/>
  🚀 DevOps • Cloud • Security Engineer <br/><br/>
  <a href="mailto:guptapiyushraj471@gmail.com"><img src="https://img.shields.io/badge/Email-guptapiyushraj471%40gmail.com-blue?logo=gmail"></a>
  <a href="https://www.linkedin.com/in/piyush-gupta-10a0546bb/"><img src="https://img.shields.io/badge/LinkedIn-Piyush%20Gupta-blue?logo=linkedin"></a>
  <a href="https://github.com/guptapiyushraj471-cpu"><img src="https://img.shields.io/badge/GitHub-guptapiyushraj471--cpu-black?logo=github"></a>
</p>

<p align="center">
  💡 *Made with ❤️ and DevSecOps project*  
</p>

------------------------------------
=======
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome"/>
  <img src="https://img.shields.io/badge/Docker-Enabled-2496ED?logo=docker&logoColor=white" alt="Docker"/>
  <img src="https://img.shields.io/badge/Kubernetes-Ready-326CE5?logo=kubernetes&logoColor=white" alt="Kubernetes"/>
  <img src="https://img.shields.io/badge/Terraform-IaC-7B42BC?logo=terraform&logoColor=white" alt="Terraform"/>
  <img src="https://img.shields.io/badge/Security-Trivy-00ADD8?logo=aqua&logoColor=white" alt="Security"/>
  <img src="https://img.shields.io/badge/AWS-EKS-FF9900?logo=amazonaws&logoColor=white" alt="AWS EKS"/>
</p>

<h3 align="center">🚀 Production-ready microservice: Node.js • Docker • Terraform • EKS • CI/CD • Security</h3>

<p align="center">
  <img src="./assets/ai-divider.png" alt="AI Divider" width="600"/>
</p>

---

# 🎯 DevOps Cloud Security Engineer Assessment

<p align="center">
  <img src="./assets/hero-image.png" alt="Hero Image" width="800"/>
</p>

**Repository:** devops-cloud-security-engineer-assessment  
**Author:** Piyush Gupta  
**Version:** 1.0.0  
**Last Updated:** December 2025

> 🤖 **AI-Powered DevOps Excellence** - A comprehensive demonstration of modern cloud-native microservice architecture with enterprise-grade security, automated CI/CD pipelines, and infrastructure as code.

---

## 📑 Table of Contents

- [🎯 DevOps Cloud Security Engineer Assessment](#-devops-cloud-security-engineer-assessment)
  - [📑 Table of Contents](#-table-of-contents)
  - [🎪 Overview](#-overview)
  - [✨ Key Features](#-key-features)
  - [🎯 Goal](#-goal)
  - [🏗️ Architecture](#️-architecture)
    - [🔧 Technology Stack](#-technology-stack)
  - [🤖 Powered by AI](#-powered-by-ai)
    - [🧠 AI Integration Points](#-ai-integration-points)
    - [🔮 AI-Enhanced Features](#-ai-enhanced-features)
  - [🗺️ Roadmap](#️-roadmap)
    - [🎯 Current Phase: v1.0 (Q4 2025)](#-current-phase-v10-q4-2025)
    - [🚀 Phase 2: Enhanced Observability (Q1 2026)](#-phase-2-enhanced-observability-q1-2026)
    - [🔮 Phase 3: Advanced Features (Q2 2026)](#-phase-3-advanced-features-q2-2026)
    - [🌟 Phase 4: AI \& Intelligence (Q3 2026)](#-phase-4-ai--intelligence-q3-2026)
    - [🎨 Phase 5: Developer Experience (Q4 2026)](#-phase-5-developer-experience-q4-2026)
  - [📸 Screenshots](#-screenshots)
    - [Application Interface](#application-interface)
    - [CI/CD Pipeline](#cicd-pipeline)
    - [Monitoring Dashboard](#monitoring-dashboard)
    - [Kubernetes Resources](#kubernetes-resources)
    - [Development Workflow](#development-workflow)
  - [📄 License](#-license)
  - [🙏 Acknowledgments](#-acknowledgments)
    - [Built With](#built-with)
  - [📞 Contact \& Support](#-contact--support)
    - [Get in Touch](#get-in-touch)
    - [Support This Project](#support-this-project)

---

## 🎪 Overview

This project showcases a **production-grade Todo microservice** built with modern DevOps practices, cloud-native technologies, and enterprise security standards. It demonstrates end-to-end automation from code commit to production deployment on AWS EKS with comprehensive monitoring, security scanning, and infrastructure as code.

<p align="center">
  <img src="./assets/tech-stack.png" alt="Technology Stack" width="700"/>
</p>

---

## ✨ Key Features

<table>
  <tr>
    <td align="center" width="33%">
      <img src="./assets/icon-cloud.png" alt="Cloud Native" width="80"/><br/>
      <b>☁️ Cloud Native</b><br/>
      Built for AWS EKS with auto-scaling, high availability, and cloud-optimized architecture
    </td>
    <td align="center" width="33%">
      <img src="./assets/icon-security.png" alt="Security First" width="80"/><br/>
      <b>🔒 Security First</b><br/>
      Trivy scanning, IRSA, non-root containers, secrets management, and threat modeling
    </td>
    <td align="center" width="33%">
      <img src="./assets/icon-automation.png" alt="Full Automation" width="80"/><br/>
      <b>⚡ Full Automation</b><br/>
      Complete CI/CD pipeline with GitHub Actions, automated testing, and deployment
    </td>
  </tr>
  <tr>
    <td align="center" width="33%">
      <img src="./assets/icon-iac.png" alt="IaC" width="80"/><br/>
      <b>🏗️ Infrastructure as Code</b><br/>
      Terraform-managed AWS resources with modular, reusable, and version-controlled infrastructure
    </td>
    <td align="center" width="33%">
      <img src="./assets/icon-monitoring.png" alt="Observability" width="80"/><br/>
      <b>📊 Observability</b><br/>
      CloudWatch integration, Prometheus-ready metrics, and comprehensive health checks
    </td>
    <td align="center" width="33%">
      <img src="./assets/icon-docker.png" alt="Containerized" width="80"/><br/>
      <b>🐳 Containerized</b><br/>
      Multi-stage Docker builds, optimized images, and Kubernetes-native deployment
    </td>
  </tr>
</table>

---

## 🎯 Goal

Create a secure, production-like Todo microservice demonstrating:
- Node.js service with `/healthz` and `/api/v1/todos` (GET, POST)
- Automated test coverage, linting
- Multi-stage Docker image (non-root)
- GitHub Actions: build → test → image → trivy scan → deploy
- Infrastructure IaC (Terraform for EKS, DynamoDB, IRSA)
- Kubernetes manifests with security best practices

---

## 🏗️ Architecture

<p align="center">
  <img src="./assets/architecture-diagram.png" alt="Architecture Diagram" width="900"/>
</p>

```mermaid
graph LR
  User --> ALB[Ingress/ALB]
  ALB --> ServiceCluster[EKS Service]
  ServiceCluster --> TodoPod[Todo App Pod]
  TodoPod --> DynamoDB[(DynamoDB Todos Table)]
  ServiceCluster -->|Metrics| CloudWatch[(CloudWatch/Prometheus)]

```

### 🔧 Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | REST API | Todo CRUD operations |
| **Runtime** | Node.js | Application runtime |
| **Container** | Docker | Application containerization |
| **Orchestration** | Kubernetes (EKS) | Container orchestration |
| **Database** | DynamoDB | Serverless NoSQL database |
| **Infrastructure** | Terraform | Infrastructure as Code |
| **CI/CD** | GitHub Actions | Automated pipelines |
| **Security** | Trivy, IRSA | Vulnerability scanning & IAM |
| **Monitoring** | CloudWatch, Prometheus | Observability & metrics |



## 🤖 Powered by AI

<p align="center">
  <img src="./assets/ai-powered-banner.png" alt="AI Powered" width="600"/>
</p>

This project leverages AI-assisted development practices and modern automation:

### 🧠 AI Integration Points

- **Code Quality**: AI-powered code review and suggestions
- **Security Analysis**: Automated vulnerability detection with Trivy
- **Documentation**: AI-assisted documentation generation
- **Testing**: Intelligent test case generation and coverage analysis
- **Infrastructure Optimization**: AI-driven resource allocation recommendations

### 🔮 AI-Enhanced Features

| Feature | Description | Status |
|---------|-------------|--------|
| 🤖 Automated Code Review | AI-powered PR analysis | ✅ Active |
| 🔍 Smart Security Scanning | ML-based threat detection | ✅ Active |
| 📊 Predictive Scaling | AI-driven auto-scaling recommendations | 🔄 In Progress |
| 🎯 Anomaly Detection | Intelligent monitoring and alerting | 🔄 In Progress |
| 📝 Auto-Documentation | AI-generated API documentation | 📅 Planned |

---

## 🗺️ Roadmap

<p align="center">
  <img src="./assets/roadmap-timeline.png" alt="Roadmap Timeline" width="900"/>
</p>

### 🎯 Current Phase: v1.0 (Q4 2025)

- [x] Core microservice implementation
- [x] CI/CD pipeline setup
- [x] Terraform infrastructure
- [x] Security scanning integration
- [x] EKS deployment
- [x] DynamoDB integration

### 🚀 Phase 2: Enhanced Observability (Q1 2026)

- [ ] Prometheus & Grafana dashboards
- [ ] Distributed tracing with Jaeger
- [ ] Advanced logging with ELK stack
- [ ] Custom metrics and alerts
- [ ] SLO/SLI implementation

### 🔮 Phase 3: Advanced Features (Q2 2026)

- [ ] Service mesh integration (Istio)
- [ ] Multi-region deployment
- [ ] Blue-green deployment strategy
- [ ] Chaos engineering tests
- [ ] Performance optimization

### 🌟 Phase 4: AI & Intelligence (Q3 2026)

- [ ] AI-powered predictive auto-scaling
- [ ] Intelligent anomaly detection
- [ ] Auto-remediation workflows
- [ ] Smart cost optimization
- [ ] AI-driven security responses

### 🎨 Phase 5: Developer Experience (Q4 2026)

- [ ] Developer portal
- [ ] Interactive API playground
- [ ] Enhanced local development tools
- [ ] One-click environment setup
- [ ] Comprehensive tutorials

---



## 📸 Screenshots

### Application Interface

<p align="center">
  <img src="./assets/screenshot-api.png" alt="API Interface" width="800"/>
  <br/>
  <em>RESTful API Endpoints</em>
</p>

### CI/CD Pipeline

<p align="center">
  <img src="./assets/screenshot-pipeline.png" alt="GitHub Actions Pipeline" width="800"/>
  <br/>
  <em>GitHub Actions Workflow Execution</em>
</p>

### Monitoring Dashboard

<p align="center">
  <img src="./assets/screenshot-monitoring.png" alt="Monitoring Dashboard" width="800"/>
  <br/>
  <em>CloudWatch Metrics & Alerts</em>
</p>

### Kubernetes Resources

<p align="center">
  <img src="./assets/screenshot-k8s.png" alt="Kubernetes Dashboard" width="800"/>
  <br/>
  <em>EKS Cluster Resources</em>
</p>

---
 Code of Conduct

Please note that this project follows a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

### Development Workflow

```mermaid
graph LR
    A[Fork Repo] --> B[Create Branch]
    B --> C[Make Changes]
    C --> D[Write Tests]
    D --> E[Run Linters]
    E --> F[Commit]
    F --> G[Push]
    G --> H[Create PR]
    H --> I[Code Review]
    I --> J[Merge]
```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 Piyush Gupta

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

---

## 🙏 Acknowledgments

Special thanks to the following projects and communities:

- **AWS** - For EKS and cloud infrastructure
- **HashiCorp** - For Terraform
- **Kubernetes** - For container orchestration
- **Docker** - For containerization platform
- **GitHub** - For CI/CD and version control
- **Trivy** - For security vulnerability scanning
- **Node.js Community** - For the runtime environment
- **Open Source Contributors** - For the amazing tools and libraries

### Built With

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"/>
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white" alt="Kubernetes"/>
  <img src="https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white" alt="Terraform"/>
  <img src="https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS"/>
  <img src="https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white" alt="GitHub Actions"/>
</p>

---

## 📞 Contact & Support

<p align="center">
  <img src="./assets/contact-banner.png" alt="Contact Banner" width="600"/>
</p>

### Get in Touch

- **Author**: Piyush Gupta
- **Repository**: [DevOps-Cloud-Security-Engineer-Assessment](https://github.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment)
- **Issues**: [Report a Bug](https://github.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment/issues)
- **Discussions**: [Start a Discussion](https://github.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment/discussions)

### Support This Project

If you find this project helpful, please consider:

- ⭐ Starring the repository
- 🐛 Reporting bugs and issues
- 💡 Suggesting new features
- 🤝 Contributing code
- 📢 Sharing with others

---

<p align="center">
  <img src="./assets/footer-banner.png" alt="Footer Banner" width="800"/>
</p>

<p align="center">
  Made with ❤️ by Piyush Gupta | Powered by ☁️ AWS & 🤖 AI
</p>

<p align="center">
  <sub>⚡ Built with modern DevOps practices • 🔒 Security-first approach • 🚀 Production-ready infrastructure</sub>
</p>

---

<p align="center">
  <a href="#-devops-cloud-security-engineer-assessment">⬆️ Back to Top</a>
</p>
>>>>>>> 0be7d04b15e58a38120331fd873ebae9c32739dc
