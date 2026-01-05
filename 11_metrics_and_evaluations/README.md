---
title: "Metrics & Evaluations"
category: must
levels: ["mid", "senior", "principal"]
skills: [accuracy, f1-score, ndcg, perplexity, rouge, bleu, llm-evals]
questions:
  "mid": ["Explain the Precision-Recall trade-off."]
  "senior": ["How do you evaluate a RAG system? What metrics are relevant?"]
  "principal": ["Design a human-in-the-loop evaluation pipeline for a code generation model."]
---

# Metrics & Evaluations

## 💡 Core Ideas

"You can't improve what you don't measure." In AI, picking the wrong metric leads to optimizing for the wrong thing.

*   **Classification**: Accuracy (dangerous for imbalanced data), Precision, Recall, F1, AUC-ROC.
*   **Regression**: MSE, MAE, R-Squared.
*   **Ranking (Search/RecSys)**: NDCG (Normalized Discounted Cumulative Gain), MAP (Mean Average Precision).
*   **Generative (LLMs)**:
    *   **N-gram based**: BLEU, ROUGE (outdated for high-quality generation).
    *   **Model-based**: BERTScore, Perplexity.
    *   **LLM-as-a-Judge**: Using GPT-4 to score Llama-2 outputs.
    *   **RAG Evals**: RAGAS (Retrieval Augmented Generation Assessment) - Faithfulness, Answer Relevance, Context Recall.

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

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: When is Accuracy a bad metric?
*   **A**: When classes are imbalanced. If 90% of patients are healthy, a model that predicts "Healthy" for everyone has 90% accuracy but is useless.

### Senior-Level
*   **Q**: Explain NDCG (Normalized Discounted Cumulative Gain).
*   **A**: Used in ranking. "Cumulative Gain" sums the relevance of results. "Discounted" penalizes relevant results if they appear lower down the list. "Normalized" divides by the ideal ranking (IDCG) so the score is between 0 and 1.

### Principal-Level
*   **Q**: We are launching a code generation assistant. How do we know it's "Good"?
*   **A**:
    1.  **Pass@k**: Can the generated code pass unit tests? (HumanEval benchmark).
    2.  **Productivity Metric**: Acceptance rate of suggestions in IDE.
    3.  **Latency**: Time to first token.
    4.  **Qualitative**: AB testing with developer focus groups.
