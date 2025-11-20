---
title: "CI/CD & MLOps"
category: ops
levels: ["mid", "senior", "principal"]
skills: [ci-cd, docker, kubernetes, mlops]
questions:
  "mid": ["What is CI/CD?", "Explain Docker vs Virtual Machine.", "What is a Git Hook?"]
  "senior": ["How to implement Blue-Green Deployment?", "Explain Kubernetes Architecture.", "How to automate Model Retraining?"]
  "principal": ["Design a full MLOps platform.", "How to handle secret management in CI/CD?", "Discuss GitOps principles (ArgoCD)."]
---

# CI/CD & MLOps

Continuous Integration/Continuous Deployment is the backbone of modern software engineering. MLOps applies these principles to Machine Learning.

## Core Concepts

### 1. CI/CD Pipeline
*   **CI (Continuous Integration)**: Developers merge code frequently. Automated tests run on every commit to prevent regressions.
*   **CD (Continuous Delivery/Deployment)**:
    *   **Delivery**: Code is ready to deploy at any time (manual approval).
    *   **Deployment**: Code is automatically deployed to production.

### 2. Containerization (Docker)
*   Packages code + dependencies + OS libraries into a single unit (Image).
*   "Build once, run anywhere." Eliminates "it works on my machine" issues.
*   **Dockerfile**: Recipe to build the image.
*   **Image**: Read-only template.
*   **Container**: Running instance of an image.

### 3. Orchestration (Kubernetes)
*   Manages thousands of containers across a cluster of machines.
*   **Pod**: Smallest unit. One or more containers sharing network/storage.
*   **Deployment**: Manages replicas of Pods (Scaling, Rolling Updates).
*   **Service**: Stable network endpoint (Load Balancer) for Pods.

## Advanced Topics

### 1. MLOps Maturity Levels (Google)
*   **Level 0**: Manual process. Script-driven. No CI/CD.
*   **Level 1**: ML Pipeline automation. Continuous Training (CT).
*   **Level 2**: CI/CD pipeline automation. Automated testing of data, model, and code.

### 2. Deployment Strategies
*   **Rolling Update**: Replace old pods with new ones gradually. Zero downtime.
*   **Blue-Green**: Spin up new version (Green) alongside old (Blue). Switch traffic instantly. Fast rollback. Expensive (double resources).
*   **Canary**: Route small % of traffic to new version. Monitor metrics. Gradually increase.

### 3. Infrastructure as Code (IaC)
*   Managing infrastructure (servers, DBs, networks) using code (Terraform, CloudFormation).
*   Version controlled, reproducible, auditable.

## Interview Questions

### Mid Level
1.  **What is the difference between Docker and a Virtual Machine?**
    *   *Answer*: VM virtualizes hardware (has its own OS kernel). Heavy. Docker virtualizes the OS (shares host kernel). Lightweight, fast startup.
2.  **What is a Multi-Stage Build in Docker?**
    *   *Answer*: A technique to keep images small. Build in one stage (with compilers/SDKs), copy only the binary/artifact to the final stage (minimal runtime).
3.  **Why do we need Unit Tests in CI?**
    *   *Answer*: To catch bugs early. If a test fails, the build fails, preventing bad code from merging.

### Senior Level
1.  **Explain the Kubernetes Architecture.**
    *   *Answer*:
        *   **Control Plane (Master)**: API Server (entry point), Etcd (store), Scheduler (assigns pods to nodes), Controller Manager.
        *   **Worker Nodes**: Kubelet (agent), Kube-proxy (network), Container Runtime (Docker/Containerd).
2.  **How does GitOps work?**
    *   *Answer*: Git is the single source of truth for infrastructure/deployment state. An agent (ArgoCD) inside the cluster pulls changes from Git and applies them (syncs state). No `kubectl apply` from laptop.
3.  **What tests should run in an ML CI/CD pipeline?**
    *   *Answer*:
        *   **Code Tests**: Unit/Integration.
        *   **Data Tests**: Schema validation, Null checks, Distribution checks.
        *   **Model Tests**: Accuracy threshold, Latency check, Bias check.

### Principal Level
1.  **Design a CI/CD pipeline for a Monorepo with 50 microservices.**
    *   *Discussion*:
        *   **Change Detection**: Only build/test services that changed (using dependency graph like Bazel/Nx).
        *   **Caching**: Cache build artifacts remotely.
        *   **Parallelism**: Run tests in parallel shards.
2.  **How to manage secrets (API Keys, Passwords) in CI/CD?**
    *   *Discussion*: NEVER commit to Git. Use Vault, AWS Secrets Manager, or GitHub Secrets. Inject them as Environment Variables at runtime.
3.  **Architect a Continuous Training (CT) pipeline.**
    *   *Discussion*:
        *   **Trigger**: Time-based (weekly) or Performance-based (drift detected).
        *   **Steps**: Ingest Data -> Validate Data -> Retrain -> Evaluate -> Register Model.
        *   **Gating**: If new model is better than current production model, promote it.
