<p align="center">
  <img src="assets/banner.svg" alt="DevOps Cloud Security Engineer Banner" width="1000" />
</p>

<p align="center">
  <img src="https://github.com/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment/actions/workflows/ci-cd.yaml/badge.svg" alt="CI Status"/>
  <img src="https://img.shields.io/github/languages/top/guptapiyushraj471-cpu/DevOps-Cloud-Security-Engineer-Assessment" alt="Language"/>
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License"/>
</p>

<h3 align="center">Production-ready microservice: Node.js • Docker • Terraform • EKS • CI/CD • Security</h3>

---


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
