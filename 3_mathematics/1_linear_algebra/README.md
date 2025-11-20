---
title: "Linear Algebra"
category: math
levels: ["mid", "senior", "principal"]
skills: [linear-algebra, matrix-decompositions, dimensionality-reduction]
questions:
  "mid": ["What is the geometric interpretation of the dot product?", "Explain Rank and Null Space.", "What is an Eigenvector?"]
  "senior": ["Explain Singular Value Decomposition (SVD) and its applications.", "How does PCA work mathematically?", "What is the condition number of a matrix?"]
  "principal": ["Derive the closed-form solution for Linear Regression (Normal Equation).", "Discuss the computational complexity of matrix inversion vs decomposition.", "Explain the connection between SVD and the Moore-Penrose Pseudoinverse."]
---

# Linear Algebra for Machine Learning

Linear Algebra is the engine of Machine Learning. Data is represented as vectors/matrices, and operations (training, inference) are linear transformations.

## Core Concepts

### 1. Vectors & Matrices
*   **Dot Product**: $\mathbf{a} \cdot \mathbf{b} = ||\mathbf{a}|| ||\mathbf{b}|| \cos \theta$. Measures similarity (projection). 0 if orthogonal.
*   **Norms**:
    *   $L_1$ (Manhattan): $\sum |x_i|$. Promotes sparsity (Lasso).
    *   $L_2$ (Euclidean): $\sqrt{\sum x_i^2}$. Used in Ridge regression.
*   **Rank**: The number of linearly independent rows/columns. Full rank = invertible (for square matrices).

### 2. Eigenvalues & Eigenvectors
*   For a square matrix $A$, if $A\mathbf{v} = \lambda \mathbf{v}$, then $\mathbf{v}$ is an eigenvector and $\lambda$ is the eigenvalue.
*   **Intuition**: Directions ($\mathbf{v}$) that do not change direction after transformation $A$, only scale by $\lambda$.
*   **Application**: PCA, Spectral Clustering, Stability analysis.

### 3. Matrix Decompositions
Decomposing a matrix into simpler constituents is crucial for numerical stability and efficiency.
*   **Eigendecomposition**: $A = Q \Lambda Q^{-1}$. Only for diagonalizable matrices.
*   **SVD (Singular Value Decomposition)**: $A = U \Sigma V^T$. Works for **any** matrix.
    *   $U$: Left singular vectors (orthogonal).
    *   $\Sigma$: Singular values (diagonal, non-negative).
    *   $V$: Right singular vectors (orthogonal).
    *   **Application**: LSA (NLP), Image Compression, Denoising.

### 4. Principal Component Analysis (PCA)
*   **Goal**: Dimensionality reduction while preserving variance.
*   **Steps**:
    1.  Center data (subtract mean).
    2.  Compute Covariance Matrix $\Sigma = \frac{1}{n} X^T X$.
    3.  Compute Eigenvectors of $\Sigma$.
    4.  Project data onto top $k$ eigenvectors.

## Advanced Topics

### 1. Matrix Calculus
Differentiation with respect to vectors/matrices.
*   **Gradient**: $\nabla_x (w^T x) = w$.
*   **Hessian**: Matrix of second derivatives. Used in Newton's method.
*   **Jacobian**: Matrix of first partial derivatives for vector-valued functions.

### 2. Positive Definite Matrices
*   A symmetric matrix $A$ is Positive Definite (PD) if $x^T A x > 0$ for all non-zero $x$.
*   **Properties**: All eigenvalues > 0. Invertible. Cholesky decomposition exists.
*   **Relevance**: Covariance matrices are always PSD. Hessian at a minimum is PD.

### 3. Ill-Conditioned Matrices
*   **Condition Number**: $\kappa(A) = \frac{|\lambda_{max}|}{|\lambda_{min}|}$.
*   **High $\kappa(A)$**: The matrix is close to singular. Small changes in input lead to huge changes in output. Numerical instability (e.g., in Linear Regression with collinear features).
*   **Fix**: Regularization (Ridge) adds $\lambda I$ to diagonal, improving condition number.

## Interview Questions

### Mid Level
1.  **What is the difference between the Dot Product and the Cross Product?**
    *   *Answer*: Dot product returns a scalar (similarity). Cross product returns a vector orthogonal to both inputs (defined in 3D).
2.  **Why is the Covariance Matrix symmetric?**
    *   *Answer*: $Cov(X, Y) = Cov(Y, X)$. The element $(i, j)$ is the covariance between feature $i$ and feature $j$.
3.  **What does it mean if the determinant of a matrix is 0?**
    *   *Answer*: The matrix is singular (non-invertible). It collapses space into a lower dimension (rank deficient).

### Senior Level
1.  **Explain the geometric interpretation of SVD.**
    *   *Answer*: Any linear transformation can be decomposed into a rotation ($V^T$), a scaling along axes ($\Sigma$), and another rotation ($U$).
2.  **How is PCA related to SVD?**
    *   *Answer*: PCA is essentially SVD on the centered data matrix. The principal components are the right singular vectors ($V$) of the data matrix $X$.
3.  **Why do we use Squared Error (L2) in Linear Regression?**
    *   *Answer*: Minimizing L2 error corresponds to Maximum Likelihood Estimation (MLE) assuming Gaussian noise. It also has a closed-form solution (differentiable everywhere).

### Principal Level
1.  **Derive the Normal Equation for Linear Regression.**
    *   *Discussion*: Start with Loss $J(w) = ||Xw - y||^2$. Expand, take derivative w.r.t $w$, set to 0. Result: $w = (X^T X)^{-1} X^T y$.
2.  **Discuss the computational complexity of solving Linear Regression via Normal Equation vs Gradient Descent.**
    *   *Discussion*: Normal Equation involves matrix inversion $(X^T X)^{-1}$, which is $O(d^3)$ where $d$ is features. Very slow for large $d$. Gradient Descent is $O(nd)$ per epoch. GD is preferred for large datasets/features.
3.  **What is the Moore-Penrose Pseudoinverse?**
    *   *Discussion*: A generalization of the inverse for non-square or singular matrices. Defined via SVD: $A^+ = V \Sigma^+ U^T$, where $\Sigma^+$ takes reciprocals of non-zero singular values. Used to solve least squares problems.
