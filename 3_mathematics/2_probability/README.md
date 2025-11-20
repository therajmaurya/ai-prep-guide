---
title: "Probability"
category: math
levels: ["mid", "senior", "principal"]
skills: [probability, bayesian-inference, information-theory]
questions:
  "mid": ["Explain Conditional Probability.", "What is Bayes' Theorem?", "Difference between PDF and CDF."]
  "senior": ["Explain MLE vs MAP.", "What is the Central Limit Theorem?", "Derive the mean of a Bernoulli distribution."]
  "principal": ["Explain the connection between KL Divergence and Cross-Entropy.", "Discuss the properties of Conjugate Priors.", "How does Variational Inference work?"]
---

# Probability for Machine Learning

Probability is the language of uncertainty. ML models are probabilistic engines that learn distributions from data.

## Core Concepts

### 1. Random Variables & Distributions
*   **Discrete**: Bernoulli (coin flip), Binomial (n flips), Poisson (events per interval).
*   **Continuous**: Uniform, Normal (Gaussian), Exponential, Beta.
*   **PDF (Probability Density Function)**: $f(x)$. Probability at a point (density).
*   **CDF (Cumulative Distribution Function)**: $F(x) = P(X \le x)$. Integral of PDF.

### 2. Bayes' Theorem
The foundation of Bayesian Inference. Updates belief based on new evidence.
$$P(A|B) = \frac{P(B|A) P(A)}{P(B)}$$
*   $P(A|B)$: Posterior (Belief after seeing data).
*   $P(B|A)$: Likelihood (Data given hypothesis).
*   $P(A)$: Prior (Initial belief).
*   $P(B)$: Evidence (Normalization constant).

### 3. Expectation & Variance
*   **Expectation (Mean)**: $E[X] = \sum x P(x)$ or $\int x f(x) dx$.
*   **Variance**: $Var(X) = E[(X - \mu)^2]$. Spread of data.
*   **Covariance**: $Cov(X, Y) = E[(X - \mu_X)(Y - \mu_Y)]$. Linear relationship.

## Advanced Topics

### 1. MLE vs MAP
Two ways to estimate parameters $\theta$ of a distribution.
*   **MLE (Maximum Likelihood Estimation)**: Maximize $P(Data | \theta)$. "What parameters make this data most probable?"
    *   Used in Frequentist statistics.
    *   Equivalent to minimizing NLL (Negative Log Likelihood).
*   **MAP (Maximum A Posteriori)**: Maximize $P(\theta | Data)$.
    *   $P(\theta | Data) \propto P(Data | \theta) P(\theta)$.
    *   Incorporates a **Prior** $P(\theta)$.
    *   Equivalent to MLE + Regularization (e.g., L2 regularization = Gaussian Prior).

### 2. Information Theory
*   **Entropy ($H$)**: Measure of uncertainty/surprise. $H(X) = - \sum p(x) \log p(x)$.
*   **KL Divergence ($D_{KL}$)**: Distance between two distributions $P$ and $Q$.
    *   $D_{KL}(P || Q) = \sum p(x) \log \frac{p(x)}{q(x)}$.
    *   Not symmetric. Always $\ge 0$.
*   **Cross-Entropy**: $H(P, Q) = H(P) + D_{KL}(P || Q)$.
    *   Minimizing Cross-Entropy (Loss) is equivalent to minimizing KL Divergence between predicted and true distribution.

### 3. Central Limit Theorem (CLT)
*   The sum (or average) of independent random variables tends toward a **Normal Distribution**, regardless of the original distribution.
*   **Why it matters**: Justifies the assumption of Gaussian noise in many models.

## Interview Questions

### Mid Level
1.  **What is the difference between Probability and Likelihood?**
    *   *Answer*: Probability attaches to possible results (data) given a fixed parameter. Likelihood attaches to parameters given fixed data.
2.  **Explain the Law of Large Numbers.**
    *   *Answer*: As sample size grows, the sample mean converges to the true population mean.
3.  **What is a Joint Probability?**
    *   *Answer*: $P(A, B)$. Probability of A and B happening together. If independent, $P(A, B) = P(A)P(B)$.

### Senior Level
1.  **Why do we use Log-Likelihood instead of Likelihood in optimization?**
    *   *Answer*: Numerical stability (avoids underflow from multiplying small probabilities) and mathematical convenience (turns products into sums, making derivatives easier).
2.  **Explain the Naive Bayes assumption.**
    *   *Answer*: Assumes features are conditionally independent given the class label. $P(x_1, x_2 | y) = P(x_1 | y) P(x_2 | y)$. Often false, but simplifies computation.
3.  **What is a Conjugate Prior?**
    *   *Answer*: If the Prior and Posterior belong to the same probability distribution family, the Prior is conjugate to the Likelihood. (e.g., Beta prior + Binomial likelihood -> Beta posterior). Makes Bayesian update closed-form.

### Principal Level
1.  **Derive the MLE for a Gaussian Distribution.**
    *   *Discussion*: Write Likelihood $L(\mu, \sigma) = \prod \frac{1}{\sqrt{2\pi}\sigma} e^{-...}$. Take Log. Differentiate w.r.t $\mu$, set to 0. Result: $\hat{\mu} = \frac{1}{n} \sum x_i$ (Sample Mean).
2.  **Explain Variational Inference (VI).**
    *   *Discussion*: Used when posterior $P(Z|X)$ is intractable. VI approximates it with a simpler distribution $Q(Z)$ by minimizing $KL(Q || P)$. Used in VAEs.
3.  **How is Logistic Regression related to Bernoulli distribution?**
    *   *Discussion*: Logistic Regression models the parameter $p$ of a Bernoulli distribution conditioned on input $x$. $y|x \sim Bernoulli(\sigma(w^T x))$. Minimizing Binary Cross Entropy is exactly MLE for this model.
