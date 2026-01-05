---
title: "Mathematics for AI"
category: must
levels: ["mid", "senior", "principal"]
skills: [linear-algebra, calculus, probability, optimization, statistics]
questions:
  "mid": ["Explain the difference between L1 and L2 regularization geometrically."]
  "senior": ["Derive the gradients for the Softmax function. Why is it often combined with Cross-Entropy loss?"]
  "principal": ["Explain the convergence properties of Adam vs SGD for non-convex optimization surfaces."]
---

# Mathematics for AI

## 💡 Core Ideas

You don't need a Math PhD, but you can't be a Senior Principal Engineer if you treat neural nets as black boxes. You need to understand the "Why" and "How" of optimization.

*   **Linear Algebra**:
    *   **Eigenvalues & Eigenvectors**: Crucial for PCA, SVD, and understanding model stability.
    *   **Matrix Decomposition**: SVD, Cholesky (used in sampling from distributions).
    *   **Tensors**: Rank, shape, broadcasting, and contraction (einsum).
*   **Calculus**:
    *   **Gradients & Jacobians**: The engine of Backpropagation.
    *   **Chain Rule**: How gradients flow through layers.
*   **Probability & Statistics**:
    *   **Bayes' Theorem**: Updating priors (foundational for generative models).
    *   **Distributions**: Gaussian, Bernoulli, Poisson, and Dirichlet (for Latent Dirichlet Allocation).
    *   **MLE vs MAP**: Maximum Likelihood vs Maximum A Posteriori estimation.
*   **Optimization**:
    *   **Convex vs Non-Convex**: Local minima vs Saddle points.
    *   **Optimizers**: SGD, Momentum, RMSProp, Adam, AdamW (weight decay separation).
    *   **Lagrange Multipliers**: Method for finding local maxima/minima subject to equality constraints (Basis of SVM dual formulation).
*   **Information Theory**:
    *   **Entropy**: Measure of uncertainty. $H(X) = - \sum p(x) \log p(x)$.
    *   **KL Divergence**: $D_{KL}(P || Q)$. The "distance" between two distributions.
    *   **Mutual Information**: $I(X;Y)$. reduction in uncertainty about X given Y. Crucial for Feature Selection.

## 📚 Resources

*   [**Mathematics for Machine Learning**](https://mml-book.com/) - The standard textbook (Free PDF available).
*   [**3Blue1Brown: Linear Algebra**](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab) - Intuitive visualizations.
*   [**Matrix Calculus for Deep Learning**](https://explained.ai/matrix-calculus/) - Great article for understanding gradients.

## 🛠️ Practice

### Project 1: NumPy Neural Net
Implement a multi-layer perceptron (MLP) from scratch using only NumPy.
*   **Goal**: Implement `forward` and `backward` passes manually. Verify gradients against PyTorch.

### Project 2: PCA from Scratch
Implement Principal Component Analysis using SVD without `sklearn`.
*   **Goal**: Visualize the variance explained by top K components on the MNIST dataset.

### Project 3: SVD Image Compression
Use Singular Value Decomposition to compress a grayscale image.
*   **Goal**: Reconstruct the image using only the top $k$ singular values and visualize the quality vs size trade-off.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the "Curse of Dimensionality"?
*   **A**: As the number of dimensions/features increases, the volume of the space increases exponentially, making the available data sparse. This makes distance-based algorithms (like KNN) ineffective and requires more data to generalize.

### Senior-Level
*   **Q**: Explain KL Divergence. Is it symmetric?
*   **A**: Kullback-Leibler Divergence measures how one probability distribution $P$ diverges from a second expected probability distribution $Q$. It is **non-symmetric** ($D_{KL}(P||Q) \neq D_{KL}(Q||P)$). It's fundamental in VAEs and distilling models.

### Principal-Level
*   **Q**: Why do Transformers need positional encodings? Explain the mathematical intuition behind Sine/Cosine embeddings.
*   **A**: Self-attention is permutation invariant (treating "Dog bites Man" same as "Man bites Dog"). Positional encodings inject order. Sine/Cosine functions allow the model to learn to attend by relative positions because $PE(pos+k)$ can be represented as a linear function of $PE(pos)$.
