# DevOps Cloud Security Engineer Assessment

**Repository:** devops-cloud-security-engineer-assessment  
**Author:** Piyush Gupta

---

## Table of contents
- [Goal](#goal)
- [Architecture](#architecture)
- [Local developer flow](#local-developer-flow)
- [CI / CD pipeline (GitHub Actions)](#ci--cd-pipeline-github-actions)
- [Infrastructure (Terraform)](#infrastructure-terraform)
- [Kubernetes manifests](#kubernetes-manifests)
- [Security, secrets & IAM](#security-secrets--iam)
- [Threat model & mitigations](#threat-model--mitigations)
- [Trade-offs & future improvements](#trade-offs--future-improvements)
- [Grader checklist (what to review)](#grader-checklist-what-to-review)

---

## Goal
Create a secure, production-like Todo microservice demonstrating:
- Node.js service with `/healthz` and `/api/v1/todos` (GET, POST)
- Automated test coverage, linting
- Multi-stage Docker image (non-root)
- GitHub Actions: build → test → image → trivy scan → deploy
- Infrastructure IaC (Terraform for EKS, DynamoDB, IRSA)
- Kubernetes manifests with security best practices

---

## Architecture

```mermaid
graph LR
  User --> ALB[Ingress/ALB]
  ALB --> ServiceCluster[EKS Service]
  ServiceCluster --> TodoPod[Todo App Pod]
  TodoPod --> DynamoDB[(DynamoDB Todos Table)]
  ServiceCluster -->|Metrics| CloudWatch[(CloudWatch/Prometheus)]
