---
title: "Cloud & Ops System Design"
category: system-design
levels: ["senior", "principal"]
skills: [kubernetes, inference-scaling, gpu-sharing, hybrid-cloud]
questions:
  "mid": ["Why use Kubernetes instead of simple EC2 instances for scaling?"]
  "senior": ["Design a system to serve a 70B LLM with <50ms TBT (Time Between Tokens)."]
  "principal": ["How do you architect a multi-region inference system that is resilient to a full zone failure?"]
---

# Cloud & Ops System Design

## 💡 Core Ideas

This section bridges the gap between pure "System Design" and "DevOps". It is about **infrastructure patterns**.

*   **Compute Patterns**:
    *   **Serverless (Lambda/Cloud Run)**: Good for intermittent CPU workloads. Bad for GPUs (cold starts).
    *   **Managed Services (SageMaker/Vertex)**: Easy to start, expensive at scale.
    *   **Kubernetes (EKS/GKE)**: The gold standard for scale. Custom auto-scaling (KEDA), GPU slicing (MIG).
*   **Storage Patterns**:
    *   **Object Storage (S3/GCS)**: The Data Lake. Cheap, high throughput, high latency.
    *   **High Performance IO**: Lustre / FSx for Lustre. Needed for distributed training to keep GPUs fed.
*   **Cost Optimization (FinOps)**:
    *   **Spot Instances**: 60-90% cheaper. Requires fault-tolerant training (checkpointing to AWS S3 / GCS).
    *   **Inference Optimization**: 
        *   **Continuous Batching (vLLM)**: To saturation GPUs.
        *   **Quantization (AWQ/GPTQ)**: Serve 70B models on single A100 instead of 2.
    *   **MIG (Multi-Instance GPU)**: Slicing a big A100 into 7 smaller instances for smaller models.
*   **Observability**:
    *   **Tracing**: OTel (OpenTelemetry) to trace a request from API -> Vector DB -> LLM.
    *   **Metrics**: Token throughput, Time to First Token (TTFT).

## 📚 Resources

*   [**Google Cloud Architecture Center**](https://cloud.google.com/architecture) - Best practices patterns.
*   [**AWS Machine Learning Lens**](https://docs.aws.amazon.com/wellarchitected/latest/machine-learning-lens/machine-learning-lens.html) - Well-Architected Framework.

## 🛠️ Practice

### Project 1: Autoscaling Inference
Deploy a dummy model on Kubernetes.
*   **Goal**: Configure **HPA** (Horizontal Pod Autoscaler) on CPU usage and **KEDA** on Request Queue Length (Prometheus metric).

### Project 2: Terraform Infrastructure
Write a Terraform script that brings up a GKE cluster with a specific GPU node pool (e.g., T4 GPUs) and installs Nvidia drivers.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: What is the difference between "Scale Up" (Vertical) and "Scale Out" (Horizontal) for LLM Inference?
*   **A**:
    *   **Vertical**: Moving to a bigger GPU (A10 -> A100). Essential if the model doesn't fit in VRAM.
    *   **Horizontal**: Adding more Replicas/Pods. Essential for handling higher QPS (Queries Per Second).

### Principal-Level
*   **Q**: We have a hybrid requirement: Sensitive data stays on-prem, but we need cloud GPUs for peak traffic. Design this.
*   **A**:
    *   **VPN/Direct Connect**: Establish secure tunnel.
    *   **Anthos / EKS Anywhere**: Run a unified K8s control plane.
    *   **Bursting**: Base load runs on private cloud. When queue depth > Threshold, spin up public cloud nodes.
    *   **Data Gravity**: Only send anonymized features to cloud, or run the model on-prem and only send weights (Federated).
