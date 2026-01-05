---
title: "Cloud & MLOps"
category: must
levels: ["mid", "senior", "principal"]
skills: [kubernetes, docker, ci/cd, monitoring, aws/gcp]
questions:
  "mid": ["Explain the purpose of a Docker multi-stage build."]
  "senior": ["How do you design a CI/CD pipeline for ML that includes model evaluation gates?"]
  "principal": ["Design a cost-effective autoscaling strategy for GPU inference services with sporadic traffic."]
---

# Cloud & MLOps

## 💡 Core Ideas

Models are useless if they stay in notebooks. MLOps is the art of shipping models reliably.

*   **Containerization**: Docker, optimizing image layers for large weights (or mounting weights as volumes).
*   **Orchestration**: Kubernetes (K8s), KServe, Ray Serve.
*   **CI/CD for ML**:
    *   **CT (Continuous Training)**: Automated retraining pipelines triggered by data drift.
    *   **Model Registry**: MLflow, Weights & Biases (W&B).
*   **Infrastructure as Code (IaC)**: Terraform, Pulumi.
*   **Monitoring**: Prometheus, Grafana, alerting on model latency and throughput.

## 📚 Resources

*   [**MLOps.org**](https://mlops.org/) - General principles.
*   [**Full Stack Deep Learning**](https://fullstackdeeplearning.com/) - Excellent course on the tooling landscape.
*   [**Kubernetes for Machine Learning**](https://www.kubeflow.org/) - Kubeflow documentation.
*   **Engineering Blogs**: [Uber Michelangelo](https://eng.uber.com/michelangelo-machine-learning-platform/), [Google TFX](https://www.tensorflow.org/tfx), [LinkedIn Feathr](https://engineering.linkedin.com/blog/2022/open-sourcing-feathr).

## 🛠️ Practice

### Project 1: Model API with Docker
Create an Inference Server using FastAPI and package it in a Docker container.
*   **Goal**: Minimize image size (use `python:slim` or `distroless`) and optimize startup time.

### Project 2: CI/CD Pipeline
Set up a GitHub Action that:
1.  Lints code.
2.  Runs unit tests.
3.  Trains a small model.
4.  Pushes metrics to a report in the PR.

### Project 3: Kubernetes Deployment
Deploy a model to a local Minikube or Kind cluster.
*   **Goal**: Create a `Deployment` and `Service` YAML. Implement a rolling update strategy.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the difference between a container and a virtual machine?
*   **A**: Containers share the host OS kernel and are lightweight, while VMs have their own full OS and kernel (via Hypervisor), making them heavier but more isolated.

### Senior-Level
*   **Q**: Discuss the Blue-Green vs Canary deployment strategy for ML models.
*   **A**:
    *   **Blue-Green**: Spin up new version (Green) alongside old (Blue). Switch traffic 100% when Green is healthy. Safe rollback, but double resource cost.
    *   **Canary**: Gradually roll out Green to 1%, 10%, etc. Good for catching "unknown unknown" issues with real traffic without affecting everyone.

### Principal-Level
*   **Q**: We spend $50k/month on GPU instances. Design a strategy to reduce this by 40% without affecting SLAs.
*   **A**:
    1.  **Spot Instances**: Use Spot instances for training (checkpoint flexible) and stateless inference (with graceful handling).
    2.  **Right-sizing**: Profile workloads; switch from A100s to A10s or T4s for inference if memory allows.
    3.  **Autoscaling**: Implement KEDA (Kubernetes Event-driven Autoscaling) based on queue depth, scale to zero at night.
    4.  **Multi-Model Serving**: Use NVIDIA Triton to serve multiple models on the same GPU.
