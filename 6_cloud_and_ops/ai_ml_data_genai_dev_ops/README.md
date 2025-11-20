---
title: "AI/ML/GenAI DevOps (MLOps)"
category: must
levels: ["senior", "principal"]
skills: [mlops, model-registry, feature-store, drift-detection, model-monitoring]
questions:
  "mid": ["What is the difference between DevOps and MLOps?", "What is Data Drift?"]
  "senior": ["Design a Model Registry workflow.", "How do you handle training-serving skew?"]
  "principal": ["Architect a feature store for a real-time fraud detection system.", "Discuss strategies for A/B testing ML models."]
---

# AI/ML/GenAI DevOps (MLOps)

MLOps is the set of practices that aims to deploy and maintain machine learning models in production reliably and efficiently. It combines Machine Learning, DevOps, and Data Engineering.

## Core Concepts

### 1. Model Registry
- **Purpose**: Central repository to store, version, and manage ML models.
- **Features**: Versioning, Lineage (which data/code produced this model), Stage transitions (Staging -> Prod), Metadata storage.
- **Tools**: MLflow Model Registry, AWS SageMaker Registry.

### 2. Feature Store
- **Purpose**: Centralized storage for features to ensure consistency between training and serving.
- **Components**:
    - **Offline Store**: High capacity, low latency (e.g., S3, BigQuery) for training.
    - **Online Store**: Low latency (e.g., Redis, DynamoDB) for real-time inference.
- **Benefits**: Prevents "Training-Serving Skew", promotes feature reuse.

### 3. Drift Detection
- **Data Drift (Covariate Shift)**: Input data distribution changes ($P(X)$ changes).
- **Concept Drift**: Relationship between input and target changes ($P(Y|X)$ changes).
- **Detection**:
    - Statistical tests: KS-test, PSI (Population Stability Index), KL Divergence.
    - Monitoring: Prometheus/Grafana, Arize AI, WhyLabs.

### 4. Continuous Training (CT)
- **Trigger**: Automated retraining when drift is detected or performance drops below a threshold.
- **Pipeline**: Data ingestion -> Validation -> Retraining -> Evaluation -> Deployment.

## Interview Questions

### Mid Level
1.  **What is the difference between DevOps and MLOps?**
    *   *Answer*: DevOps focuses on code and infrastructure. MLOps adds Data and Models as first-class citizens. MLOps involves experimental code, non-deterministic outcomes, and the need for continuous retraining (CT).
2.  **What is Data Drift? Give an example.**
    *   *Answer*: When the statistical properties of the input data change. Example: A fraud model trained on pre-COVID spending patterns failing during the pandemic because spending habits changed.

### Senior Level
1.  **How do you handle "Training-Serving Skew"?**
    *   *Answer*: Ensure the exact same feature engineering code is used for training and inference. Use a Feature Store to serve pre-computed features. Log prediction requests and compare their distribution with training data.
2.  **Design a deployment strategy for a new model version.**
    *   *Answer*:
        - **Shadow Mode**: Run new model in parallel with old, log predictions but don't serve them to user. Compare performance.
        - **Canary**: Roll out to 1% of traffic. Monitor errors/latency.
        - **A/B Test**: Split traffic 50/50 to measure business impact (CTR, conversion).
        - **Blue/Green**: Switch traffic fully after verification.

### Principal Level
1.  **Architect a Feature Store for a high-scale real-time recommendation system.**
    *   *Discussion*: Ingestion layer (Kafka/Flink) for real-time features. Batch ingestion (Spark) for historical features. Online store (Redis) for <10ms retrieval. Offline store (Iceberg/Delta Lake) for point-in-time correct training datasets. Feature Registry for discovery.
2.  **How would you implement "Feedback Loops" in an MLOps pipeline?**
    *   *Discussion*: Capture user actions (clicks, views) as ground truth labels. Join these labels with the prediction logs (using RequestID). Store this "labeled dataset" for future retraining. Handle delayed feedback (e.g., conversion happens days later).
