---
title: "AI System Design"
category: must
levels: ["mid", "senior", "principal"]
skills: [architecture, scalability, trade-offs, serving, monitoring]
questions:
  "mid": ["Design a system to classify images uploaded by users in real-time."]
  "senior": [
    "Design a YouTube-like video recommendation system. Handle 1B+ users.",
    "Design an LLM-based customer support agent for multi-tenant SaaS. Discuss prompt routing, caching, and safety."
  ]
  "principal": ["Design a global, multi-region feature store for a financial trading platform with <10ms latency."]
---

# AI System Design

## 💡 Core Ideas

System Design for AI differs from traditional software system design because you must account for **data dynamics, model retraining, and non-deterministic behavior**.

*   **Training Pipelines**: Batch vs streaming data, data versioning, and feature engineering pipelines.
*   **Serving Patterns**: 
    *   **Real-time (Online)**: REST/gRPC API, immediate inference.
    *   **Batch (Offline)**: Pre-compute predictions for all users.
    *   **Streaming**: Event-driven inference (Kafka/Flink).
    *   **Edge**: Running models on device (iOS/Android/IoT).
*   **Data Strategy**: Lakehouse architectures, Feature Stores (Feast/Tecton), and handling training-serving skew.
*   **Infrastructure**: CPU vs GPU inference, autoscaling, and cost optimization (Spot instances).

## 📚 Resources

*   [**Machine Learning System Design Interview**](https://github.com/alirezadir/Machine-Learning-Interview/blob/main/contents/system-design.md) - A great community resource.
*   [**Designing Machine Learning Systems**](https://www.oreilly.com/library/view/designing-machine-learning/9781098107956/) - Book by Chip Huyen (Highly Recommended).
*   [**Google's Rules of Machine Learning**](https://developers.google.com/machine-learning/guides/rules-of-ml) - Best practices from Google.

## 🛠️ Practice

### Project 1: End-to-End Prediction Service
Build a simple API using FastAPI that serves a SciKit-Learn model.
*   **Goal**: Dockerize it, add a `/health` endpoint, and add Prometheus metrics for latency and request count.

### Project 2: Feature Store Simulation
Implement a basic "Feature Store" using Redis (online) and Parquet/S3 (offline).
*   **Goal**: Write a script to "materialize" features from offline to online store.

### Project 3: Multimodal Inference System
Design a low-latency inference system for a model that takes both Image and Text as input.
*   **Goal**: Produce a component diagram focusing on the "pre-processing" (image resizing) vs "model inference" split. Discuss where to run the image processing (CPU vs GPU).

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: When would you choose Batch Inference over Real-time Inference?
*   **A**: Choose Batch when immediate results aren't required (e.g., weekly email recommendations), cost needs to be minimized, or throughput is high but latency sensitivity is low.

### Senior-Level
*   **Q**: How do you handle Training-Serving Skew?
*   **A**: Skew happens when training data distribution differs from live data.
    1.  **Schema Skew**: Verify data types match.
    2.  **Feature Logic Skew**: Ensure same code computes features in training and serving (use a Feature Store).
    3.  **Distribution Skew**: Monitor drift using tools like Evidently AI or Alibi Detect.

### Principal-Level
*   **Q**: We need to deploy a 100B+ parameter LLM for an internal chatbot. It's too slow and expensive. Optimize it.
*   **A**:
    1.  **Quantization**: Use 4-bit/8-bit quantization (GPTQ, AWQ).
    2.  **Distillation**: Train a smaller student model.
    3.  **Speculative Decoding**: Use a small draft model to generate tokens, verified by the large model.
    4.  **Hardware**: Use vLLM or TGI for continuous batching and PagedAttention to maximize GPU throughput.
