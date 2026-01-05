---
title: "Optimization in Deep Learning"
category: dl
levels: ["mid", "senior", "principal"]
skills: [optimizers, learning-rate-schedules, loss-landscapes, regularization]
questions:
  "mid": ["Explain the difference between SGD and Adam."]
  "senior": ["Why do we need Learning Rate Warmup?"]
  "principal": ["Discuss the relationship between Batch Size, Learning Rate, and Generalization Gap."]
---

# Optimization in Deep Learning

## 💡 Core Ideas

Optimization is the engine of Deep Learning. It's not just about finding *any* minimum, but finding a minimum that **generalizes**.

*   **First-Order Methods**:
    *   **SGD (Stochastic Gradient Descent)**: The baseline. Noisy but generalizes well.
    *   **Momentum**: Accumulates velocity to power through flat regions and dampen oscillations.
    *   **Adaptive Methods (RMSProp, Adam)**: Adapt learning rates per parameter. Faster convergence but can settle in sharp minima (worse generalization).
    *   **AdamW**: Adam with decoupled weight decay (the correct way to do L2 regularization in adaptive methods).
*   **Second-Order Methods**:
    *   **L-BFGS**: Uses Hessian approximation. Great for small problems, too expensive for deep learning.
    *   **Shampoo**: Preconditioner based on matrix roots. Used in large-scale Google models.
*   **Loss Landscapes**:
    *   **Sharp vs Flat Minima**: Flat minima tend to generalize better because they are robust to shifts in the test data distribution.
    *   **Saddle Points**: The main enemy in high dimensions (not local minima).
*   **Learning Rate Schedules**:
    *   **Cosine Annealing**: Smooth decay.
    *   **Cycle**: Cyclic learning rates to hop out of local minima.
    *   **Warmup**: Linearly increasing LR at the start to stabilize training (essential for Transformers).

*   **Convex vs Non-Convex Optimization**.
*   **Regularization effects on optimization**.

## 📚 Resources

*   [**An Overview of Gradient Descent Optimization Algorithms**](https://ruder.io/optimizing-gradient-descent/) - Sebastian Ruder's classic blog post.
*   [**Visualizing the Loss Landscape of Neural Nets**](https://arxiv.org/abs/1712.09913) - Analyzing why Skip Connections make landscapes smoother.
*   [**Don't Decay the Learning Rate, Increase the Batch Size**](https://arxiv.org/abs/1711.00489) - Scaling laws for optimization.
*   [Convex Optimization (Boyd)](https://web.stanford.edu/~boyd/cvxbook/)

## 🛠️ Practice

### Project 1: Vizualizing Trajectories
Train a simple 2-layer net on MNIST.
*   **Goal**: Plot the 2D trajectory of weights during training for SGD vs Adam. (Use PCA to project the high-dim weights to 2D).

### Project 2: Implementing AdamW
Implement the AdamW optimizer from scratch in PyTorch (subclassing `torch.optim.Optimizer`).
*   **Goal**: Match the official implementation's behavior.

### Project 3: Implement Adam optimizer from scratch.
### Project 4: Visualize the loss landscape of a simple neural network.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the difference between Momentum and Nesterov Momentum?
*   **A**: Standard Momentum calculates the gradient at the current position and adds the velocity. Nesterov "looks ahead" by calculating the gradient *after* applying the velocity step, leading to better correction.

### Senior-Level
*   **Q**: Why does AdamW generalize better than Adam?
*   **A**: In Adam, L2 regularization is implemented as adding `w` to the gradient. Because Adam scales gradients by the running variance, the regularization strength implicitly changes over time. AdamW decouples this, applying weight decay *directly* to the weights, independent of the adaptive scaling.
*   **Q**: Discuss the convergence properties of SGD.
*   **A**: SGD converges to a local minimum, but the path taken can be unstable and sensitive to initialization. It's the most basic method but works well in practice.
*   **Q**: What is the relationship between Batch Size, Learning Rate, and Generalization Gap?
*   **A**: Batch Size controls the amount of information used in each update. Larger batches provide more information but are more unstable. Learning Rate controls the step size. Too small and training takes forever; too large and it overshoots. Generalization Gap is the difference between training and test performance. It's minimized by finding a balance between Batch Size and Learning Rate.

### Principal-Level
*   **Q**: You are training a massive model (100B params). Shuffling the dataset (10TB) globally is too expensive. How does this affect optimization?
*   **A**:
    *   **Problem**: Inadequate shuffling leads to correlation in batches, causing the gradient effectively to oscillate or drift, hurting convergence.
    *   **Mitigation**:
        1.  **WebDataset / Sharding**: Shuffle shards, then shuffle within an in-memory buffer.
        2.  **Gradient Accumulation**: Aggregating gradients can sometimes average out noise from correlated samples.
        3.  **Curriculum Learning**: Intentionally sorting data (easy to hard) might actually help if done right.
