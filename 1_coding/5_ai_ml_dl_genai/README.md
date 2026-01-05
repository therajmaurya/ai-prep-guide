---
title: "Coding Patterns for AI/ML"
category: coding
levels: ["mid", "senior"]
skills: [pytorch, numpy, broadcasting, vectorization, einsum]
questions:
  "mid": ["What is Broadcasting?"]
  "senior": ["Rewrite this for-loop matrix multiplication using `einsum`."]
  "principal": ["Optimize a custom CUDA kernel integration."]
---

# Coding Patterns for AI/ML

## 💡 Core Ideas

Writing "Pythonic" AI code means thinking in Tensors.

*   **Broadcasting**: The magic rule that allows operations on arrays of different shapes.
    *   *Rule*: Dimensions are compatible if they are equal or one of them is 1. (Right-aligned).
*   **Vectorization**: Replacing explicit for-loops with array operations. Exploits SIMD instructions on CPU and parallel cores on GPU.
*   **Einstein Summation (`einsum`)**: A domain-specific language for tensor contractions.
    *   `torch.einsum('ik,kj->ij', A, B)` is Matrix Multiplication.
    *   `torch.einsum('ik,kj->ij', A, B)` is Matrix Multiplication.
    *   `torch.einsum('ii->', A)` is Trace.
    *   `torch.einsum('bhid,bhjd->bhij', Q, K)` is Multi-Head Attention Score calculation.
    *   `torch.einsum('bij,bjk->bik', A, B)` is Batch Matrix Multiplication.
*   **Views vs Copies**: Understanding `contiguous()` memory. Use `reshape()` with care.
*   **Standard Training Loops**: Forward, Loss, Backward, Step.
*   **Checkpointing**: Saving/loading models.
*   **Inference Pipelines**: Inference pipelines.
*   **Metric Calculation**: Metric calculation during training.

## 🛠️ Practice

### Project 1: Einsum Challenge
Implement Dot Product, Outer Product, Matrix Mul, and Attention mechanism using **only** `torch.einsum`.

### Project 2: Broadcasting Visualization
Create a visual explainer (using Matplotlib) of how a `(3, 1)` array adds to a `(1, 4)` array to produce a `(3, 4)` array.

### Project 3: Reusable Training Loop
Write a class `Trainer` that accepts a `model`, `optimizer`, `loss_fn`, and `dataloader`.
*   **Goal**: Handle GPU transfer, `model.train()`/`eval()` toggling, and average loss calculation per epoch. Add a `predict` method.

### Project 4: Early Stopping logic
Implement a class `EarlyStopping` that tracks validation loss.
*   **Goal**: If loss doesn't improve for `patience` epochs, stop training and restore best weights. Handle `modes` (min/max) and `min_delta` (threshold).

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: Why is `for` loop slow in Python?
*   **A**: Python is interpreted. Type checking and dynamic dispatch overhead happen at *every iteration*. C++/CUDA moves the loop to the compiled level.
*   **Q**: How to handle gradient accumulation?
*   **A**: Gradient accumulation is used to simulate a larger batch size when memory is limited. It accumulates gradients over multiple mini-batches before updating the model parameters.
*   **Q**: What is the purpose of `model.eval()`?
*   **A**: `model.eval()` sets the model to evaluation mode, which disables dropout and batch normalization layers.

### Senior-Level
*   **Q**: Predict output shape: `A = (10, 1, 5)`, `B = (5,)`. What is `A + B`?
*   **A**: `B` becomes `(1, 1, 5)` -> broadcasts to `(10, 1, 5)`. Result: `(10, 1, 5)`.
*   **Q**: Design a plugin-based trainer that supports callbacks.
*   **A**: Implement a trainer class that allows adding custom callbacks for different training stages (e.g., before training, after epoch, before/after batch).

