---
title: "Coding & Algorithms for AI"
category: must
levels: ["mid", "senior", "principal"]
skills: [python-optimization, numpy, pytorch-internals, custom-ops, profilers]
questions:
  "mid": ["How do you optimize a slow Python data loading loop for PyTorch?"]
  "senior": ["Explain the memory layout of strided arrays in NumPy/PyTorch and how it affects performance."]
  "principal": ["Design a custom CUDA kernel for a novel attention mechanism. What are the trade-offs vs using composed primitives?"]
---

# Coding & Algorithms for AI

## 💡 Core Ideas

This section isn't just about LeetCode. It's about **engineering performance**. As a Senior+ AI Engineer, you must understand how your code interacts with hardware (CPU/GPU/TPU).

*   **Python Performance**: CPython Global Interpreter Lock (GIL), multiprocessing vs multithreading, and async IO for data ingestion.
*   **Vectorization**: Broadcasting rules, avoiding for-loops, and using `einsum` for readable tensor operations.
*   **Memory Management**: Contiguous vs non-contiguous memory, pinned memory, and garbage collection quirks in deep learning frameworks.
*   **Numerical Stability**: Floating point precision (FP32 vs FP16 vs BF16), underflow/overflow, and log-sum-exp trick.
*   **Profiling**: Using tools like `cProfile`, `py-spy`, and PyTorch Profiler to find bottlenecks.

## 📚 Resources

*   [**High Performance Python**](https://www.oreilly.com/library/view/high-performance-python/9781492055013/) - Excellent book on optimizing Python.
*   [**PyTorch Performance Tuning Guide**](https://pytorch.org/tutorials/recipes/recipes/tuning_guide.html) - Official guide from PyTorch.
*   [**NumPy Internals**](https://numpy.org/doc/stable/reference/internals.html) - Understanding strides and memory layout.
*   [**What every computer scientist should know about floating-point arithmetic**](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html) - Essential reading.

## 🛠️ Practice

### Project 1: Custom Auto-grad Engine
Implement a tiny scalar-value autograd engine (like Karpathy's micrograd) to understand the DAG (Directed Acyclic Graph) construction and backpropagation.
*   **Goal**: Implement `Value` class with `__add__`, `__mul__`, and `backward`.

### Project 2: High-Performance Data Loader
Write a custom `Dataset` and `DataLoader` in pure Python that reads efficiently from disk without blocking the GPU.
*   **Goal**: Compare simple file reading vs using `multiprocessing` with shared memory.

### Project 3: Profiling Hunt
Take a deliberately slow model training script (add random sleeps or use unoptimized tensor ops) and use PyTorch Profiler to identify and fix the bottlenecks.

### Quiz: Efficient Vector Similarity
**Constraint**: Implement a function to compute pairwise cosine similarity between two large sets of vectors `A` (MxD) and `B` (NxD) **without using a for-loop**.
*   **Hint**: Use broadcasting and matrix multiplication (`A @ B.T`). Watch out for memory usage if M and N are huge!

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the difference between `view()` and `reshape()` in PyTorch?
*   **A**: `view()` returns a new tensor with the same data but different shape; it requires the data to be contiguous. `reshape()` may return a copy if the data is not contiguous or a view if it is.

### Senior-Level
*   **Q**: How does Automatic Mixed Precision (AMP) works? What are the risks?
*   **A**: AMP uses FP16 for matmuls/convolutions for speed and FP32 for accumulations (like gradients) for stability. It uses loss scaling to prevent gradient underflow. Risks include numerical instability if not tuned correctly.

### Principal-Level
*   **Q**: We are seeing significant GPU idle time during training. How would you debug and fix this? (Systematic approach)
*   **A**: This indicates a Data Loading bottleneck or CPU-bound preprocessing. 
    1.  **Analyze**: Check CPU/GPU utilization plots.
    2.  **Profile**: Use PyTorch Profiler to see `DataLoader` wait times.
    3.  **Fix**: Increase `num_workers`, use `pin_memory=True`, pre-fetch data, move augmentation to GPU (`kornia` or DALI), or use a faster storage format (TFRecord/WebDataset).
