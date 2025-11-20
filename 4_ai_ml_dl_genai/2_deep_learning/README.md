---
title: "Deep Learning"
category: dl
levels: ["mid", "senior", "principal"]
skills: [deep-learning, neural-networks, backpropagation, architectures]
questions:
  "mid": ["Explain Backpropagation.", "What is Dropout?", "Difference between Sigmoid and ReLU."]
  "senior": ["Explain Batch Norm vs Layer Norm.", "Why do ResNets work?", "How does the Attention mechanism work?"]
  "principal": ["Discuss the initialization of deep networks (Xavier/He).", "Explain the Lottery Ticket Hypothesis.", "How to train a model with 100B parameters?"]
---

# Deep Learning

Deep Learning is the subset of ML based on artificial neural networks with representation learning. It powers modern AI (Vision, NLP, Speech).

## Core Concepts

### 1. Neural Networks (MLP)
*   **Perceptron**: $y = \sigma(Wx + b)$.
*   **Activation Functions**:
    *   **Sigmoid**: $\frac{1}{1+e^{-x}}$. Output (0, 1). Vanishing gradient problem.
    *   **ReLU**: $\max(0, x)$. Solves vanishing gradient. Dead ReLU problem.
    *   **Softmax**: Converts logits to probabilities. Used in multi-class classification.

### 2. Backpropagation
*   The algorithm to compute gradients of the loss w.r.t weights.
*   Uses the **Chain Rule** of calculus.
*   **Forward Pass**: Compute loss.
*   **Backward Pass**: Propagate error backwards layer by layer.

### 3. Regularization
*   **Dropout**: Randomly zeroing out neurons during training. Prevents co-adaptation of features. Acts as an ensemble.
*   **Weight Decay**: L2 Regularization on weights.
*   **Early Stopping**: Stop training when validation loss starts increasing.

## Advanced Topics

### 1. Architectures
*   **CNN (Convolutional Neural Networks)**:
    *   **Convolution**: Local connectivity, parameter sharing (translation invariance).
    *   **Pooling**: Downsampling (Max/Avg).
    *   **ResNet**: Skip connections ($y = F(x) + x$) allow gradients to flow through identity path, enabling very deep networks.
*   **RNN/LSTM/GRU**: For sequential data. Handle variable length inputs.
*   **Transformers**: Attention-based. Parallelizable (unlike RNNs). State of the art.

### 2. Normalization
*   **Batch Normalization**: Normalizes activations across the batch dimension. Stabilizes training, allows higher LR. Dependent on batch size.
*   **Layer Normalization**: Normalizes across the feature dimension (for a single sample). Independent of batch size. Preferred for RNNs and Transformers.

### 3. Optimization
*   **SGD with Momentum**: Accelerates in relevant direction, dampens oscillations.
*   **Adam**: Adaptive learning rates.
*   **Learning Rate Schedulers**: Cosine Annealing, Warmup. Crucial for convergence.

## Interview Questions

### Mid Level
1.  **Why is ReLU better than Sigmoid?**
    *   *Answer*: 1) Computationally efficient (simple threshold). 2) Reduces Vanishing Gradient problem (gradient is 1 for $x>0$). 3) Induces sparsity.
2.  **What is the purpose of Max Pooling?**
    *   *Answer*: Reduces spatial dimensions (downsampling), reduces parameters/computation, and provides translation invariance.
3.  **Explain the difference between Epoch and Batch.**
    *   *Answer*: Epoch: One pass through the entire dataset. Batch: A subset of data processed at once.

### Senior Level
1.  **Why do we need Skip Connections in ResNets?**
    *   *Answer*: They solve the degradation problem in deep networks. They allow the gradient to flow directly through the identity mapping during backprop, preventing it from vanishing. This makes the loss surface smoother.
2.  **Explain the Attention Mechanism ($Q, K, V$).**
    *   *Answer*: Attention computes a weighted sum of values ($V$) based on the similarity between query ($Q$) and keys ($K$). $Attention(Q, K, V) = softmax(\frac{QK^T}{\sqrt{d_k}})V$. It allows the model to focus on relevant parts of the input.
3.  **What is the difference between Batch Norm and Layer Norm?**
    *   *Answer*: BN normalizes across the batch (N) for each channel (C). LN normalizes across channels (C) for each sample (N). BN breaks if batch size is 1; LN works fine.

### Principal Level
1.  **Explain Xavier (Glorot) and He Initialization.**
    *   *Discussion*:
        *   **Goal**: Keep variance of activations and gradients constant across layers.
        *   **Xavier**: For Sigmoid/Tanh. $Var(W) = \frac{2}{n_{in} + n_{out}}$.
        *   **He**: For ReLU. $Var(W) = \frac{2}{n_{in}}$.
        *   *Why*: If weights are too small, signal vanishes. If too large, signal explodes (or saturates).
2.  **Discuss the "Lottery Ticket Hypothesis".**
    *   *Discussion*: A randomly initialized dense network contains a subnet (winning ticket) that, when trained in isolation, matches the accuracy of the original network. Implies that most weights are unnecessary (pruning).
3.  **How do you train a model that is larger than GPU memory?**
    *   *Discussion*:
        *   **Gradient Checkpointing**: Trade compute for memory.
        *   **Mixed Precision (FP16)**: Halves memory usage.
        *   **Model Parallelism**: Split layers across GPUs.
        *   **ZeRO (Zero Redundancy Optimizer)**: Shard optimizer states.
        *   **Offloading**: Move optimizer states to CPU RAM (NVMe).
