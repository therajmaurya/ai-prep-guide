---
title: "Monitoring & Observability"
category: cloud-ops
levels: ["senior", "principal"]
skills: [prometheus, grafana, data-drift, model-drift, alerting]
questions:
  "mid": ["What is the difference between Logging and Metrics?"]
  "senior": ["How do you detect Data Drift in production without labels?"]
  "principal": ["Design an observability stack for a multi-cloud ML platform."]
---

# Monitoring & Observability

## 💡 Core Ideas

"Observability" is understanding the internal state of a system based on its outputs. For ML, this is 3-dimensional:
1.  **System Metrics**: CPU, RAM, Latency (P50/P99), Error Rate.
2.  **Data Quality**: Null rates, Schema changes, Feature distributions.
3.  **Model Quality**: Prediction distribution, Bias drift, Accuracy (if feedback loop exists).

*   **Three Pillars**:
    *   **Logs**: Discrete events (JSON).
    *   **Metrics**: Aggregatable numbers (Counts, Gauges, Histograms).
    *   **Traces**: Request lifecycle across microservices.
*   **Drift Detection**:
    *   **Covariate Shift**: $P(X)$ changes, $P(y|X)$ stays same.
    *   **Concept Drift**: $P(y|X)$ changes (the world changes).
    *   **Methods**: K-S Test, PSI (Population Stability Index), KL Divergence.

*   **Service Level Objectives (SLOs) and SLIs**.
*   **Distributed Tracing (OpenTelemetry)**:
    *   Standard for generating and collecting telemetry data.
    *   **Context Propagation**: Passing a `TraceID` across microservices (API -> Vector DB -> LLM).
    *   **Spans**: Individual units of work (e.g., "DB Query: 50ms").

### 4. LLM Observability
*   **Token Counters**: Input vs Output tokens (different costs).
*   **Latency Breakdown**: TTFT (Time To First Token) vs TBT (Time Between Tokens).
*   **Cost Monitoring**: Real-time $ tracking per tenant.

## 📚 Resources

*   [**Prometheus Documentation**](https://prometheus.io/docs/introduction/overview/) - The standard for metrics.
*   [**Evidently AI**](https://github.com/evidentlyai/evidently) - Open source ML monitoring.
*   [**Google SRE Book**](https://sre.google/sre-book/table-of-contents/)

## 🛠️ Practice

### Project 1: Grafana Dashboard
Set up a Prometheus/Grafana stack using Docker Compose. Instrument a Python script to push a custom counter metric `prediction_count` and a histogram `inference_latency`.
*   **Goal**: Create a dashboard visualizing these metrics.

### Project 2: Drift Detector
Simulate data drift. Train a model on "Month 1" data. Generate "Month 2" data with a shifted mean.
*   **Goal**: Use `scipy.stats.ks_2samp` to detect the feature drift automatically.

### Project 3: Alerting
Set up a Prometheus/Grafana stack using Docker Compose. Instrument a Python script to push a custom counter metric `prediction_count` and a histogram `inference_latency`.
*   **Goal**: Create a dashboard visualizing these metrics.

### Project 4: Implement distributed tracing using OpenTelemetry.
*   **Goal**: Implement distributed tracing using OpenTelemetry.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: How to detect model drift in production?
*   **A**: Use `scipy.stats.ks_2samp` to detect the feature drift automatically.

*   **Q**: What is the "Feedback Loop" problem in ML monitoring?
*   **A**: In many cases (e.g., Fraud, Credit), you don't know the ground truth label immediately (delayed feedback). You might know weeks later.
    *   **Solution**: Proxy metrics. Monitor output distribution drift. If the model starts predicting "Fraud" 50% of the time instead of 1%, alarm immediately, even without labels.

### Principal-Level
*   **Q**: How do you monitor a RAG system?
*   **A**:
    1.  **Retrieval Metrics**: Number of documents retrieved, Similarity scores distribution.
    2.  **Generation Metrics**: Length of response, Token usage, Hallucination rate (using LLM-as-a-Judge).
    3.  **User Metrics**: Thumbs up/down, "Copy to clipboard" rate.
