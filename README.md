<p align="center">
  <img src="https://raw.githubusercontent.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment/main/assets/banner.svg"
       alt="DevOps Cloud Security Engineer Banner"
       width="1000" />
</p>

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

## 🧾 Table of contents
- [Overview](#-overview)  
- [Demo / Screenshots](#-demo--screenshots)  
- [Tech Stack](#-tech-stack)  
- [Architecture](#-architecture)  
- [Quickstart (local)](#-quickstart-local)  
- [CI / CD](#-ci--cd)  
- [Infrastructure (Terraform)](#-infrastructure-terraform)  
- [Kubernetes Manifests](#-kubernetes-manifests)  
- [Security & Hardening](#-security--hardening)  
- [Testing & Validation](#-testing--validation)  
- [Grader Checklist / Mapping](#-grader-checklist--mapping)  
- [Future Work](#-future-work)  
- [Contact](#-contact)

---

## 👋 Overview
This repository is a **complete submission** for a DevOps & Cloud Security Engineer assessment.  
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

### 🧠 1. EKS Pod Egress Spike (Incident Response)
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
