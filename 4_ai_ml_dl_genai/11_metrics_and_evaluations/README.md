---
title: "Metrics and Evaluations"
category: ml
levels: ["mid", "senior", "principal"]
skills: [metrics, evaluation, ab-testing, accuracy, f1-score, ndcg, perplexity, rouge, bleu, llm-evals]
questions:
  "mid": ["Difference between Precision and Recall.", "What is F1 Score?", "Explain RMSE vs MAE.", "Explain the Precision-Recall trade-off."]
  "senior": ["Explain ROC-AUC and PR-AUC.", "What is LogLoss (Cross-Entropy)?", "How to evaluate a RAG system?", "Explain NDCG."]
  "principal": ["Discuss metrics for Generative AI (FID, BLEU, ROUGE).", "How to handle offline vs online metric mismatch?", "Design a composite metric for user satisfaction.", "Design a human-in-the-loop evaluation pipeline for a code generation model."]
---

# Metrics and Evaluations

"You can't improve what you don't measure." Choosing the right metric is often more important than choosing the right model. In AI, picking the wrong metric leads to optimizing for the wrong thing.

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
*   **MAP (Mean Average Precision)**: Focuses on precision at all levels of recall.

## Advanced Topics

### 1. Probabilistic Metrics
*   **LogLoss (Binary Cross Entropy)**: Penalizes confident wrong predictions heavily.
    *   $Loss = - [y \log(p) + (1-y) \log(1-p)]$.
*   **ROC-AUC**: Area Under the Receiver Operating Characteristic Curve. Plot of TPR vs FPR at various thresholds. Probability that a random positive example is ranked higher than a random negative example. Robust to class imbalance.
*   **PR-AUC**: Area Under Precision-Recall Curve. Better than ROC for extremely imbalanced datasets (needle in haystack).

### 2. Generative AI Metrics
*   **NLP (Text - N-gram based)**:
    *   **BLEU**: N-gram overlap precision. Used for Translation.
    *   **ROUGE**: N-gram overlap recall. Used for Summarization.
    *   **Perplexity**: Exponentiated Cross-Entropy. How surprised the model is by the text.
*   **NLP (Model-based & Semantics)**:
    *   **BERTScore**: Computes similarity using contextual embeddings.
    *   **LLM-as-a-Judge**: Using stronger models (e.g., GPT-4) to score outputs from smaller models (e.g., Llama-2).
*   **Vision (Images)**:
    *   **FID (Fréchet Inception Distance)**: Measures distance between feature distributions of real and generated images. Lower is better.
    *   **IS (Inception Score)**: Measures quality and diversity.

### 3. RAG Evaluation (RAGAS)
*   **Faithfulness**: (GenAI) Is the answer derived *only* from the context? (Hallucination check).
*   **Answer Relevance**: (GenAI) Does the answer address the query?
*   **Context Precision**: (Retrieval) Is the *relevant* chunk ranked at the top?
*   **Context Recall**: (Retrieval) Did we retrieve the ground truth chunk?

### 4. Code Generation Metrics
*   **Pass@k**: Generate $k$ solutions. If *any* of them passes the unit tests, it's a success.
    *   $Pass@k = 1 - \frac{\binom{n-c}{k}}{\binom{n}{k}}$ where $n$ is total samples, $c$ is correct samples.
*   **Bleu code**: Not recommended. Code can be correct but syntactically different.


## 📚 Resources

*   [**Scikit-Learn Metrics**](https://scikit-learn.org/stable/modules/model_evaluation.html) - Documentation.
*   [**RAGAS**](https://github.com/explodinggradients/ragas) - Framework for evaluating RAG pipelines.
*   [**HuggingFace Evaluate**](https://huggingface.co/docs/evaluate/index) - Library for metric calculation.

## 🛠️ Practice

### Project 1: Balanced Metrics
Train a classifier on a highly imbalanced dataset (e.g., Credit Card Fraud, 99.9% legit).
*   **Goal**: Show that Accuracy is 99.9% but Recall is 0%. Fix it by optimizing for F1 or Recall@K.

### Project 2: LLM Evaluation Bench
Use `RAGAS` to evaluate a simple Wiki-Chatbot.
*   **Goal**: Generate a synthetic test set (Questions, Ground Truths) and run evaluation metrics.

### Project 3: Fairness Audit
Take a pre-trained model (e.g., for hiring or credit scoring) and run an error analysis on different subgroups (gender, age).
*   **Goal**: Calculate "Demographic Parity" and "Equalized Odds" metrics. Propose a mitigation strategy (re-weighting or post-processing).

## Interview Questions

### Mid Level
1.  **When should you use F1 Score over Accuracy?**
    *   *Answer*: When classes are imbalanced. A model predicting "No Fraud" 99.9% of the time has 99.9% accuracy but 0 F1 score for the Fraud class.
2.  **What does an AUC of 0.5 mean?**
    *   *Answer*: Random guessing. The model has no discriminative power.
3.  **Why is RMSE sensitive to outliers?**
    *   *Answer*: Because it squares the errors. An error of 10 becomes 100. An error of 0.1 becomes 0.01.
4.  **Explain the Precision-Recall trade-off.**
    *   *Answer*: Increasing threshold increases Precision (fewer FPs) but decreases Recall (more FNs). Decreasing threshold increases Recall but decreases Precision. You can't usually maximize both simultaneously.
5.  **When is Accuracy a bad metric?**
    *   *Answer*: When classes are imbalanced. If 90% of patients are healthy, a model that predicts "Healthy" for everyone has 90% accuracy but is useless.

### Senior Level
1.  **Explain the difference between Micro-F1 and Macro-F1.**
    *   *Answer*: Macro calculates F1 for each class and averages them (treats all classes equally). Micro calculates global TP, FP, FN and computes F1 (dominated by majority class). Use Macro if minority class performance matters.
2.  **Why is LogLoss better than Accuracy for training?**
    *   *Answer*: Accuracy is a step function (not differentiable). LogLoss is smooth and convex (differentiable), providing a gradient for optimization. It also quantifies *confidence*.
3.  **What are the limitations of BLEU score?**
    *   *Answer*: It only measures n-gram overlap. It doesn't capture semantic meaning, synonyms, or fluency. "The cat sat on the mat" and "Mat the sat cat on the" might have similar BLEU scores.
4.  **Explain NDCG (Normalized Discounted Cumulative Gain).**
    *   *Answer*: Used in ranking. "Cumulative Gain" sums the relevance of results. "Discounted" penalizes relevant results if they appear lower down the list. "Normalized" divides by the ideal ranking (IDCG) so the score is between 0 and 1.

### Principal Level
1.  **How do you handle the mismatch between Offline Metrics (AUC) and Online Metrics (Click-Through Rate)?**
    *   *Discussion*: Offline metrics are proxies. If AUC goes up but CTR goes down, investigate: 1) Distribution shift (training data $\ne$ live data). 2) Latency (better model is too slow). 3) UI changes. Run A/B tests to validate.
2.  **Design a metric for "Diversity" in a Recommender System.**
    *   *Discussion*: Intra-List Similarity (ILS). Average cosine similarity between all pairs of items in the recommendation list. We want to minimize ILS (maximize diversity) without sacrificing relevance.
3.  **Explain FID (Fréchet Inception Distance) mathematically.**
    *   *Discussion*: It assumes features (from InceptionV3 pool3 layer) follow a multidimensional Gaussian. $FID = ||\mu_r - \mu_g||^2 + Tr(\Sigma_r + \Sigma_g - 2(\Sigma_r \Sigma_g)^{1/2})$. Measures distance between Real ($\mu_r, \Sigma_r$) and Generated ($\mu_g, \Sigma_g$) distributions.
4.  **We are launching a code generation assistant. How do we know it's "Good"?**
    *   *Discussion*: 
        1.  **Pass@k**: Can the generated code pass unit tests? (HumanEval benchmark).
        2.  **Productivity Metric**: Acceptance rate of suggestions in IDE.
        3.  **Latency**: Time to first token.
        4.  **Qualitative**: AB testing with developer focus groups.
