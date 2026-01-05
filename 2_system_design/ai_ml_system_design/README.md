---
title: "AI/ML System Design"
category: system-design
levels: ["senior", "principal"]
skills: [ml-system-design, mlops, serving, monitoring]
questions:
  "senior": ["Design a Recommendation System (YouTube).", "How to handle Training/Serving Skew?", "Explain Feature Store."]
  "principal": ["Design a Real-time Fraud Detection System.", "Architect a system for continuous training (CT).", "How to serve LLMs at scale?"]
---

# AI/ML System Design

Designing ML systems requires bridging the gap between Data Science (Models) and Engineering (Scalability, Reliability).

## Core Components

### 1. Data Pipeline
*   **Ingestion**: Batch (Airflow/Spark) vs Streaming (Kafka/Flink).
*   **Feature Engineering**: Transforming raw data into model inputs.
*   **Feature Store**: Centralized repo for features. Ensures consistency between training and serving.

### 2. Training Pipeline
*   **Model Selection**: Choosing the right algorithm.
*   **Hyperparameter Tuning**: Grid Search, Random Search, Bayesian Optimization.
*   **Experiment Tracking**: MLflow, Weights & Biases. Reproducibility is key.

### 3. Inference Pipeline
*   **Batch Prediction**: Run model on millions of records nightly. Save results to DB.
*   **Online Prediction**: Real-time API. Needs low latency.
*   **Edge Deployment**: Running on device (TensorFlow Lite, ONNX).

## Advanced Topics

### 1. Training/Serving Skew
*   Performance degrades in production because data distribution changes.
*   **Schema Skew**: Training data has different columns than serving data.
*   **Distribution Skew**: Concept Drift (User behavior changes) or Covariate Shift (Input distribution changes).

### 2. Model Monitoring
*   **Data Drift**: Monitoring statistical properties of inputs (KL Divergence).
*   **Model Drift**: Monitoring prediction distribution.
*   **Ground Truth Lag**: We might not know if a prediction was correct until days later (e.g., Loan Default).

### 3. Serving Strategies
*   **Shadow Mode**: Deploy new model alongside old. Log predictions but don't show to user. Compare performance.
*   **Canary Deployment**: Roll out to 1% of users. Monitor errors.
*   **A/B Testing**: Split traffic 50/50. Compare business metrics.

### 4. Agentic AI Design
Designing systems where the LLM takes actions (Calls APIs).
*   **ReAct Pattern**: Reason + Act. Model outputs a thought, then an action, observes result, then reasons again.
*   **Memory Systems**: 
    *   **Short-term**: Context window.
    *   **Long-term**: Vector DB (RAG) or Summarized history.
*   **Planning**: Breaking complex goals into sub-tasks (Chain of Density, Tree of Thoughts).
*   **Guardrails**: Input/Output filtering (NeMo Guardrails, Guardrails AI) to prevent dangerous actions.

### 5. Evaluation Systems (Evals)
*   **Unit Tests for AI**: Fixed inputs with known good outputs.
*   **LLM-as-a-Judge**: Using GPT-4 to grade the output of a smaller model (Llama-3).
*   **RAGAS Metrics**: Faithfulness, Answer Relevance, Context Precision.

## Interview Questions

### Senior Level
1.  **Design a Recommendation System for YouTube.**
    *   *Discussion*:
        *   **Candidate Generation (Retrieval)**: Fast, cheap model (Two-Tower, Matrix Factorization) to select top 1000 videos from billions.
        *   **Ranking**: Heavy, accurate model (Deep Neural Network) to sort the 1000 candidates.
        *   **Re-ranking**: Business logic (remove duplicates, filter NSFW).
2.  **What is a Feature Store and why do we need it?**
    *   *Answer*: It solves the "Training/Serving Skew" problem. It provides a single source of truth for features. "Point-in-time correctness" for training (Time Travel) and low-latency access for serving (Redis).

### Principal Level
1.  **Design a Real-time Fraud Detection System.**
    *   *Discussion*:
        *   **Latency**: Must decide in < 200ms.
        *   **Features**: Aggregations (count of txns in last 10 mins). Computed by Flink/Spark Streaming.
        *   **Model**: XGBoost or GNN.
        *   **Feedback Loop**: Human reviewers label suspicious cases. Active Learning to update model.
2.  **How do you handle "Feedback Loops" in ML systems?**
    *   *Discussion*: The model's predictions influence the data it will be trained on next. (e.g., RecSys only shows what users like -> Filter Bubble).
    *   *Fix*: Exploration (Epsilon-Greedy). Randomly show unseen items to gather unbiased data.
3.  **Architect a system for LLM Serving.**
    *   *Discussion*:
        *   **Compute**: H100 GPUs.
        *   **Optimization**: KV Cache, Continuous Batching (vLLM), Quantization (AWQ/GPTQ).
        *   **Speculative Decoding**: Use small model to draft, large model to verify.
