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

<p align="center">
  <img src="https://skillicons.dev/icons?i=nodejs,docker,kubernetes,terraform,aws,githubactions,github" />
</p>

---

## 🏗️ Architecture

```mermaid
graph TD
A[User] --> B[Ingress Controller]
B --> C[Service]
C --> D[Pod: Node.js API]
D --> E[DynamoDB]
C --> F[Prometheus Metrics]

