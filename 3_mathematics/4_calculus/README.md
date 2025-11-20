---
title: "Calculus"
category: math
levels: ["mid", "senior", "principal"]
skills: [calculus, optimization, gradients]
questions:
  "mid": ["What is a derivative?", "Explain the Chain Rule.", "What is Gradient Descent?"]
  "senior": ["Explain the Jacobian and Hessian matrices.", "How does Adam optimizer work?", "What is the Vanishing Gradient problem?"]
  "principal": ["Derive Backpropagation for a simple MLP.", "Discuss Second-Order Optimization methods (Newton's Method).", "Explain Lagrange Multipliers for constrained optimization."]
---

# Calculus for Machine Learning

Calculus is the study of change. In ML, it is used to optimize models (minimize loss) by finding the direction of steepest descent.

## Core Concepts

### 1. Derivatives & Gradients
*   **Derivative**: Rate of change of a function at a point. Slope of the tangent line. $f'(x) = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$.
*   **Gradient ($\nabla f$)**: Vector of partial derivatives. Points in the direction of steepest ascent. $\nabla f = [\frac{\partial f}{\partial x_1}, \dots, \frac{\partial f}{\partial x_n}]^T$.

### 2. The Chain Rule
The backbone of Backpropagation.
*   If $y = f(u)$ and $u = g(x)$, then $\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx}$.
*   In Deep Learning: Loss $L$ depends on output $y$, which depends on weights $w$.
    $$\frac{\partial L}{\partial w} = \frac{\partial L}{\partial y} \cdot \frac{\partial y}{\partial w}$$

### 3. Optimization
*   **Global Minima**: The absolute lowest point of the function.
*   **Local Minima**: A point lower than its immediate neighbors.
*   **Saddle Point**: A point where gradient is 0 but it's a min in one direction and max in another. Major problem in high-dimensional optimization.

## Advanced Topics

### 1. Jacobian & Hessian
*   **Jacobian ($J$)**: Matrix of first-order partial derivatives for a vector-valued function $\mathbf{f}: \mathbb{R}^n \to \mathbb{R}^m$.
    *   $J_{ij} = \frac{\partial f_i}{\partial x_j}$.
*   **Hessian ($H$)**: Square matrix of second-order partial derivatives for a scalar function $f: \mathbb{R}^n \to \mathbb{R}$.
    *   $H_{ij} = \frac{\partial^2 f}{\partial x_i \partial x_j}$.
    *   Describes the **curvature** of the loss landscape.

### 2. Gradient Descent Variants
*   **SGD (Stochastic Gradient Descent)**: Updates weights using one sample (or mini-batch). Noisy but escapes local minima.
*   **Momentum**: Accumulates past gradients to smooth out updates and accelerate convergence.
*   **Adam (Adaptive Moment Estimation)**: Combines Momentum (1st moment) and RMSProp (2nd moment - adaptive learning rate). Standard for DL.

### 3. Constrained Optimization
*   **Lagrange Multipliers**: To minimize $f(x)$ subject to $g(x) = 0$, we minimize the Lagrangian $\mathcal{L}(x, \lambda) = f(x) + \lambda g(x)$.
*   Used in SVM derivation (Dual problem).

## Interview Questions

### Mid Level
1.  **What is the physical meaning of the gradient?**
    *   *Answer*: It points in the direction of the greatest rate of increase of the function. To minimize a function, we move in the opposite direction ($-\nabla f$).
2.  **Why do we need activation functions?**
    *   *Answer*: To introduce non-linearity. Without them, a neural network is just a linear regression model (stack of linear layers = one linear layer).
3.  **Explain Learning Rate.**
    *   *Answer*: A hyperparameter that controls the step size during optimization. Too small = slow convergence. Too large = overshooting/divergence.

### Senior Level
1.  **What is the Vanishing Gradient problem?**
    *   *Answer*: In deep networks with Sigmoid/Tanh activations, gradients become very small during backprop (chain rule multiplication of small numbers). Lower layers stop learning.
    *   *Fix*: Use ReLU, Residual Connections (ResNet), Batch Normalization.
2.  **Explain the difference between Convex and Non-Convex optimization.**
    *   *Answer*: Convex functions have only one global minimum (bowl shape). Gradient Descent is guaranteed to find it. Neural Networks are Non-Convex (many local minima/saddle points).
3.  **How does L2 Regularization relate to Calculus?**
    *   *Answer*: Adding $\lambda ||w||^2$ to the loss adds a term $2\lambda w$ to the gradient. This is "Weight Decay" - it forces weights to be small.

### Principal Level
1.  **Derive Backpropagation for a Linear Layer.**
    *   *Discussion*: $y = Wx + b$. Given $\frac{\partial L}{\partial y}$, find $\frac{\partial L}{\partial W}$ and $\frac{\partial L}{\partial x}$.
        *   $\frac{\partial L}{\partial W} = \frac{\partial L}{\partial y} x^T$.
        *   $\frac{\partial L}{\partial x} = W^T \frac{\partial L}{\partial y}$.
2.  **Discuss Newton's Method for optimization.**
    *   *Discussion*: Uses second-order information (Hessian) to jump directly to the minimum (assuming quadratic function). Update: $x_{new} = x_{old} - H^{-1} \nabla f$.
    *   *Pros*: Fast convergence near minimum.
    *   *Cons*: Computing and inverting Hessian ($O(N^3)$) is too expensive for Deep Learning.
3.  **Why does Adam work better than SGD?**
    *   *Discussion*: Adam adapts the learning rate for each parameter individually. It reduces the learning rate for parameters with large gradients (high variance) and increases it for sparse parameters. It combines the benefits of Momentum (speed) and RMSProp (stability).
