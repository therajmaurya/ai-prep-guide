---
title: "Core Machine Learning"
category: ml
levels: ["mid", "senior", "principal"]
skills: [supervised-learning, unsupervised-learning, scikit-learn, classical-ml]
questions:
  "mid": ["Explain the Bias-Variance Tradeoff.", "What is Regularization (L1 vs L2)?", "How does K-Means clustering work?"]
  "senior": ["Explain the Kernel Trick in SVM.", "Difference between Bagging and Boosting.", "How to handle imbalanced datasets?"]
  "principal": ["Derive the update rule for Gradient Boosting.", "Discuss the convergence properties of K-Means.", "Explain t-SNE and UMAP for dimensionality reduction."]
---

# Core Machine Learning

Before Deep Learning took over, these "Classical ML" algorithms ruled the world. They are still superior for tabular data (XGBoost/LightGBM) and small datasets.

## Core Concepts

### 1. Supervised Learning
*   **Regression**: Predicting continuous values (Linear Regression, Ridge/Lasso).
*   **Classification**: Predicting discrete labels (Logistic Regression, SVM, Decision Trees).

### 2. Unsupervised Learning
*   **Clustering**: Grouping similar data points (K-Means, DBSCAN, Hierarchical).
*   **Dimensionality Reduction**: Compressing data while preserving structure (PCA, t-SNE).

### 3. Support Vector Machines (SVM)
*   **Goal**: Find the hyperplane that maximizes the **margin** between classes.
*   **Support Vectors**: The data points closest to the hyperplane. Only these points matter for the model.
*   **Kernel Trick**: Mapping data to a higher-dimensional space where it becomes linearly separable, without actually computing the coordinates (using dot products).
    *   Kernels: Linear, Polynomial, RBF (Radial Basis Function).

### 4. Ensemble Methods
Combining multiple weak learners to create a strong learner.
*   **Bagging (Bootstrap Aggregating)**: Train models in parallel on random subsets of data. Reduces Variance.
    *   *Example*: Random Forest.
*   **Boosting**: Train models sequentially. Each model corrects the errors of the previous one. Reduces Bias.
    *   *Example*: AdaBoost, Gradient Boosting (XGBoost, LightGBM, CatBoost).

## Advanced Topics

### 1. Gradient Boosting Machines (GBM)
*   **Concept**: Fit a new model $h_t(x)$ to the **residuals** (negative gradient of loss) of the previous ensemble $F_{t-1}(x)$.
*   $F_t(x) = F_{t-1}(x) + \eta h_t(x)$.
*   **XGBoost**: Optimized implementation with regularization, tree pruning, and hardware acceleration.

### 2. Clustering Algorithms
*   **K-Means**: Centroid-based. Assumes spherical clusters. Sensitive to outliers. Requires specifying K.
*   **DBSCAN**: Density-based. Finds core points and expands clusters. Can find arbitrary shapes. Robust to outliers (noise). No need to specify K (needs epsilon).

### 3. Imbalanced Datasets
*   **Resampling**: Oversampling minority class (SMOTE) or Undersampling majority class.
*   **Class Weights**: Penalize mistakes on minority class more heavily in the loss function.
*   **Metrics**: Do NOT use Accuracy. Use Precision, Recall, F1-Score, or AUC-ROC.

## Interview Questions

### Mid Level
1.  **What is the difference between L1 and L2 Regularization?**
    *   *Answer*: L1 (Lasso) adds $\lambda |w|$. Promotes sparsity (feature selection). L2 (Ridge) adds $\lambda w^2$. Shrinks weights towards zero but not exactly zero.
2.  **How does a Decision Tree decide where to split?**
    *   *Answer*: It maximizes Information Gain (or minimizes Entropy/Gini Impurity). It chooses the feature and threshold that best separates the classes.
3.  **What is Cross-Validation?**
    *   *Answer*: Splitting data into K folds. Train on K-1, validate on 1. Repeat K times. Gives a robust estimate of model performance.

### Senior Level
1.  **Explain the Random Forest algorithm.**
    *   *Answer*: It's an ensemble of Decision Trees trained via Bagging. Key features: 1) Bootstrap sampling of data. 2) Random subset of features considered at each split (decorrelates trees).
2.  **Why is XGBoost often better than Random Forest?**
    *   *Answer*: Boosting reduces Bias (can model complex patterns better), whereas Bagging mainly reduces Variance. XGBoost also has regularization terms that RF lacks.
3.  **How does SMOTE work?**
    *   *Answer*: Synthetic Minority Over-sampling Technique. It creates synthetic examples by interpolating between existing minority class samples (picking a neighbor and choosing a point on the line connecting them).

### Principal Level
1.  **Derive the objective function for Logistic Regression.**
    *   *Discussion*: Start with Bernoulli likelihood. Take negative log. Result is Binary Cross Entropy Loss: $-\sum [y \log(\hat{y}) + (1-y) \log(1-\hat{y})]$.
2.  **Compare t-SNE and PCA.**
    *   *Discussion*: PCA is linear, preserves global structure (variance), deterministic. t-SNE is non-linear, preserves local structure (neighbors), probabilistic/stochastic. t-SNE is better for visualization, PCA for feature engineering.
3.  **How would you debug a model that has high training error (High Bias)?**
    *   *Discussion*: The model is underfitting. Solutions: 1) Increase model complexity (deeper trees, more layers). 2) Add more features. 3) Reduce regularization. 4) Train longer.
