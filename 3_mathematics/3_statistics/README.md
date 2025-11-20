---
title: "Statistics"
category: math
levels: ["mid", "senior", "principal"]
skills: [statistics, hypothesis-testing, ab-testing, experimentation]
questions:
  "mid": ["What is a P-value?", "Type I vs Type II Error.", "Explain Confidence Intervals."]
  "senior": ["Design an A/B test.", "Explain the Bias-Variance Tradeoff.", "What is Bootstrapping?"]
  "principal": ["How to handle interference (network effects) in A/B tests?", "Discuss Multiple Comparison Problem (Bonferroni correction).", "Explain Causal Inference (Correlation != Causation)."]
---

# Statistics for Data Science

Statistics provides the tools to validate models and interpret data. It bridges the gap between raw numbers and actionable insights.

## Core Concepts

### 1. Hypothesis Testing
*   **Null Hypothesis ($H_0$)**: The default assumption (e.g., "No effect", "Coin is fair").
*   **Alternative Hypothesis ($H_1$)**: The claim we want to test (e.g., "Treatment works").
*   **P-value**: The probability of observing data *at least as extreme* as the current observation, assuming $H_0$ is true.
    *   $p < 0.05$: Reject $H_0$ (Statistically Significant).
    *   $p \ge 0.05$: Fail to reject $H_0$.

### 2. Errors
*   **Type I Error ($\alpha$)**: False Positive. Rejecting $H_0$ when it is true. (Convicting an innocent person).
*   **Type II Error ($\beta$)**: False Negative. Failing to reject $H_0$ when it is false. (Letting a guilty person go).
*   **Power ($1 - \beta$)**: Probability of correctly rejecting $H_0$ (detecting an effect if it exists).

### 3. A/B Testing
The gold standard for product decision making.
1.  **Randomization**: Split users randomly into Control (A) and Treatment (B).
2.  **Metric**: Define a primary metric (e.g., Conversion Rate).
3.  **Sample Size**: Calculate required N to achieve desired Power (usually 80%) and Significance Level (5%).
4.  **Test**: Run experiment.
5.  **Analyze**: Use T-test or Z-test to compare means.

## Advanced Topics

### 1. Bias-Variance Tradeoff
Fundamental problem in supervised learning.
*   **Bias**: Error due to overly simple assumptions (Underfitting). High bias = Misses the pattern.
*   **Variance**: Error due to sensitivity to noise in training data (Overfitting). High variance = Models noise.
*   **Goal**: Minimize Total Error = $Bias^2 + Variance + Irreducible Error$.

### 2. Resampling Methods
*   **Bootstrapping**: Sampling *with replacement* from the dataset to estimate the sampling distribution of a statistic (e.g., Confidence Interval of the Median).
*   **Cross-Validation**: Splitting data into K folds to estimate model performance on unseen data.

### 3. Causal Inference
*   **Correlation**: X and Y move together.
*   **Causation**: X causes Y.
*   **Confounder**: A variable Z that influences both X and Y, creating a spurious correlation.
*   **Randomized Controlled Trial (RCT)**: The only way to prove causation (A/B testing is an RCT).

## Interview Questions

### Mid Level
1.  **What is a Confidence Interval?**
    *   *Answer*: A range of values derived from sample statistics that is likely to contain the true population parameter. "95% CI" means if we repeated the experiment 100 times, the interval would capture the true mean 95 times.
2.  **When should you use Median instead of Mean?**
    *   *Answer*: When the distribution is skewed or has outliers (e.g., Income). Mean is sensitive to outliers; Median is robust.
3.  **What is the difference between Z-test and T-test?**
    *   *Answer*: Z-test is used when population variance is known (or N > 30). T-test is used when population variance is unknown and N < 30.

### Senior Level
1.  **How do you determine the sample size for an A/B test?**
    *   *Answer*: Using Power Analysis. Depends on:
        1.  **Baseline Rate** (Current conversion).
        2.  **MDE (Minimum Detectable Effect)**: Smallest improvement we care about (e.g., 1%).
        3.  **Power** (usually 0.8).
        4.  **Significance Level** ($\alpha$, usually 0.05).
2.  **Explain the Multiple Comparison Problem.**
    *   *Answer*: If you test 20 hypotheses with $\alpha=0.05$, you expect 1 False Positive by chance ($20 \times 0.05 = 1$).
    *   *Fix*: Bonferroni Correction (divide $\alpha$ by N) or False Discovery Rate (FDR).
3.  **What is Simpson's Paradox?**
    *   *Answer*: A trend appears in different groups of data but disappears or reverses when these groups are combined. Caused by a confounding variable.

### Principal Level
1.  **How do you handle Network Effects (Interference) in A/B testing?**
    *   *Discussion*: In social networks, Treatment on User A affects User B (Control). Standard randomization fails (SUTVA violation).
    *   *Fix*: Cluster-based randomization (randomize cities or communities, not individuals) or Switchback experiments (randomize time slots).
2.  **Explain the difference between Frequentist and Bayesian statistics.**
    *   *Discussion*: Frequentist: Parameters are fixed constants, data is random. Uses P-values. Bayesian: Parameters are random variables with distributions. Uses Priors and Posteriors.
3.  **Design a metric to measure "User Engagement" that is robust to gaming.**
    *   *Discussion*: Avoid simple counts (clicks). Use composite metrics like "Time Spent" + "Retention" + "Meaningful Interaction". Discuss Goodhart's Law ("When a measure becomes a target, it ceases to be a good measure").
