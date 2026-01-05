---
title: "Terraform for MLOps"
category: cloud-ops
levels: ["senior", "principal"]
skills: [terraform, iac, aws-sagemaker, gcp-vertex]
questions:
  "mid": ["What is Infrastructure as Code (IaC)?"]
  "senior": ["How do you manage state in a team environment using Terraform?"]
  "principal": ["Architect a modular Terraform library for vending ML environments to data science teams."]
---

# Terraform for MLOps

## 💡 Core Ideas

Clicking around in the AWS Console is not "Engineering". Infrastructure must be reproducible, version-controlled, and auditable.

*   **Declarative Syntax (HCL)**: You describe the *desired state* ("I want 3 GPU nodes"), not the steps to get there.
Imperative IAC**: You describe the *steps to get there* ("Create an EC2 instance, then install Kubernetes, then deploy the app").
*   **State Management**: `terraform.tfstate`. Tracks the mapping between your code and real-world resources. MUST be stored remotely (S3/GCS) with locking (DynamoDB).
*   **Modules**: Reusable blueprints (e.g., a "Standard Training Cluster" module).
*   **Providers**: Plugins that talk to APIs (AWS, Azure, Kubernetes, Databricks, Pinecone).
*   **Terraform Resource Blocks**: Define infrastructure objects like a server, database or network (e.g., `aws_instance`, `k8s_deployment`).
*   **Terraform Data Blocks**: Define data sources (e.g., `aws_ami`, `k8s_cluster`).
*   **Terraform Output Blocks**: Define outputs (e.g., `aws_instance`, `k8s_deployment`).
Other Blocks: 
*   **Terraform Variable Blocks**: Define variables (e.g., `aws_instance`, `k8s_deployment`).
*   **Terraform Module Blocks**: Define modules (e.g., `aws_instance`, `k8s_deployment`).
*   **Terraform Provider Blocks**: Define providers (e.g., `aws_instance`, `k8s_deployment`).


## 📚 Resources

*   [**Terraform Registry**](https://registry.terraform.io/) - Pre-built modules.
*   [**MLOps with Terraform**](https://github.com/aws-samples/amazon-sagemaker-mlops-with-terraform) - AWS Sample.

## 🛠️ Practice

### Project 1: S3 Bucket for Datasets
Write a Terraform script to provision an S3 bucket with:
1.  Versioning enabled.
2.  Lifecycle policy (move to Glacier after 30 days).
3.  Server-side encryption.

### Project 2: SageMaker Notebook Instance
Provision a SageMaker Notebook instance attached to a specific IAM role and Security Group using Terraform.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: What is the difference between "Drift" and "State" in Terraform?
*   **A**: "Drift" refers to the state of your infrastructure deviating from your code. "State" refers to Terraform's tracking of your infrastructure.
*   **Q**: What happens if you manually delete an EC2 instance that was created by Terraform?
*   **A**: Terraform's state assumes it still exists. On the next `terraform plan`, it will fail to refresh the state (404 Not Found) or detect it's missing (Drift). It will then propose to **re-create** the missing resource to match the declarative code.

### Principal-Level
*   **Q**: Discuss strategies for drift detection in IaC.
*   **A**: Implement regular `terraform plan` checks, use tools like `terraform-compliance`, and set up alerts for drift.
*   **Q**: Design a CI/CD pipeline for Infrastructure.
*   **A**:
    1.  **PR**: Developer changes HCL code.
    2.  **Check**: `terraform fmt`, `tflint`.
    3.  **Plan**: CI runs `terraform plan` and posts the output as a comment on the PR.
    4.  **Policy**: OPA (Open Policy Agent) checks plan for security risks (e.g., "No open S3 buckets").
    5.  **Merge**: CI runs `terraform apply`.
