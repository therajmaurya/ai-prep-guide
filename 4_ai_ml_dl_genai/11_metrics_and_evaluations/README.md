---
title: "Metrics and Evaluations"
category: ml
levels: ["mid", "senior", "principal"]
skills: [metrics, evaluation, ab-testing]
questions:
  "mid": ["Difference between Precision and Recall.", "What is F1 Score?", "Explain RMSE vs MAE."]
  "senior": ["Explain ROC-AUC and PR-AUC.", "What is LogLoss (Cross-Entropy)?", "How to evaluate a RAG system?"]
  "principal": ["Discuss metrics for Generative AI (FID, BLEU, ROUGE).", "How to handle offline vs online metric mismatch?", "Design a composite metric for user satisfaction."]
---

# Metrics and Evaluations

"You can't improve what you don't measure." Choosing the right metric is often more important than choosing the right model.

## Core Concepts

### 1. Classification Metrics
*   **Accuracy**: Correct / Total. Bad for imbalanced data.
*   **Precision**: TP / (TP + FP). "Of all predicted positives, how many are real?" (Spam detection).
*   **Recall (Sensitivity)**: TP / (TP + FN). "Of all real positives, how many did we find?" (Cancer detection).
*   **F1 Score**: Harmonic mean of Precision and Recall. $2 \cdot \frac{P \cdot R}{P + R}$.

### 2. Regression Metrics
*   **MAE (Mean Absolute Error)**: Average magnitude of errors. Robust to outliers.
*   **MSE (Mean Squared Error)**: Penalizes large errors quadratically. Differentiable.
*   **RMSE**: Root MSE. Interpretable (same units as target).
*   **R-Squared ($R^2$)**: Proportion of variance explained by the model.

### 3. Ranking Metrics
*   **MRR (Mean Reciprocal Rank)**: Rank of the first relevant item.
*   **NDCG (Normalized Discounted Cumulative Gain)**: Accounts for position and relevance score. Standard for Search/RecSys.

## Advanced Topics

### 1. Probabilistic Metrics
*   **LogLoss (Binary Cross Entropy)**: Penalizes confident wrong predictions heavily.
    *   $Loss = - [y \log(p) + (1-y) \log(1-p)]$.
*   **ROC-AUC**: Area Under the Receiver Operating Characteristic Curve. Plot of TPR vs FPR at various thresholds. Probability that a random positive example is ranked higher than a random negative example. Robust to class imbalance.
*   **PR-AUC**: Area Under Precision-Recall Curve. Better than ROC for extremely imbalanced datasets (needle in haystack).

### 2. Generative AI Metrics
*   **NLP (Text)**:
    *   **BLEU**: N-gram overlap precision. Used for Translation.
    *   **ROUGE**: N-gram overlap recall. Used for Summarization.
    *   **Perplexity**: Exponentiated Cross-Entropy. How surprised the model is by the text.
*   **Vision (Images)**:
    *   **FID (Fréchet Inception Distance)**: Measures distance between feature distributions of real and generated images. Lower is better.
    *   **IS (Inception Score)**: Measures quality and diversity.

### 3. RAG Evaluation
*   **RAGAS**: Framework for evaluating RAG.
    *   **Faithfulness**: Is the answer derived from the context?
    *   **Answer Relevance**: Does the answer address the query?
    *   **Context Precision**: Is the retrieved context relevant?

## Interview Questions

### Mid Level
1.  **When should you use F1 Score over Accuracy?**
    *   *Answer*: When classes are imbalanced. A model predicting "No Fraud" 99.9% of the time has 99.9% accuracy but 0 F1 score for the Fraud class.
2.  **What does an AUC of 0.5 mean?**
    *   *Answer*: Random guessing. The model has no discriminative power.
3.  **Why is RMSE sensitive to outliers?**
    *   *Answer*: Because it squares the errors. An error of 10 becomes 100. An error of 0.1 becomes 0.01.

### Senior Level
1.  **Explain the difference between Micro-F1 and Macro-F1.**
    *   *Answer*: Macro calculates F1 for each class and averages them (treats all classes equally). Micro calculates global TP, FP, FN and computes F1 (dominated by majority class). Use Macro if minority class performance matters.
2.  **Why is LogLoss better than Accuracy for training?**
    *   *Answer*: Accuracy is a step function (not differentiable). LogLoss is smooth and convex (differentiable), providing a gradient for optimization. It also quantifies *confidence*.
3.  **What are the limitations of BLEU score?**
    *   *Answer*: It only measures n-gram overlap. It doesn't capture semantic meaning, synonyms, or fluency. "The cat sat on the mat" and "Mat the sat cat on the" might have similar BLEU scores.

### Principal Level
1.  **How do you handle the mismatch between Offline Metrics (AUC) and Online Metrics (Click-Through Rate)?**
    *   *Discussion*: Offline metrics are proxies. If AUC goes up but CTR goes down, investigate: 1) Distribution shift (training data $\ne$ live data). 2) Latency (better model is too slow). 3) UI changes. Run A/B tests to validate.
2.  **Design a metric for "Diversity" in a Recommender System.**
    *   *Discussion*: Intra-List Similarity (ILS). Average cosine similarity between all pairs of items in the recommendation list. We want to minimize ILS (maximize diversity) without sacrificing relevance.
3.  **Explain FID (Fréchet Inception Distance) mathematically.**
    *   *Discussion*: It assumes features (from InceptionV3 pool3 layer) follow a multidimensional Gaussian. $FID = ||\mu_r - \mu_g||^2 + Tr(\Sigma_r + \Sigma_g - 2(\Sigma_r \Sigma_g)^{1/2})$. Measures distance between Real ($\mu_r, \Sigma_r$) and Generated ($\mu_g, \Sigma_g$) distributions.
