---
title: "C++ for AI"
category: languages
levels: ["senior", "principal"]
skills: [cpp, cuda, tensorrt, onnx-runtime, libtorch]
questions:
  "senior": ["What is RAII (Resource Acquisition Is Initialization)?", "How do you pass a Tensor from Python to C++ without copying?"]
  "principal": ["Write a Custom autograd function in C++ for PyTorch.", "Optimize a Matrix Multiplication kernel using Tiling and AVX intrinsics."]
---

# C++ for AI Engineering

## 💡 Core Ideas

While Python is the *User Interface* of AI, C++ is the *Engine*. Core libraries (PyTorch, TensorFlow, NumPy) are written in C/C++.

*   **Custom Operators**: Implementing novel layers that don't exist in standard PyTorch.
*   **High-Performance Serving**: Running models in C++ (LibTorch, ONNX Runtime, TensorRT) to remove Python overhead.
*   **Edge AI**: Running models on microcontrollers or embedded devices.

## 📚 Key Libraries

*   [**LibTorch**](https://pytorch.org/cppdocs/): The C++ frontend for PyTorch. Same API style.
*   [**TensorRT**](https://developer.nvidia.com/tensorrt): NVIDIA's high-performance inference optimizer.
*   [**ONNX Runtime**](https://onnxruntime.ai/): Cross-platform inference accelerator.
*   [**Eigen**](https://eigen.tuxfamily.org/): Template library for linear algebra.

## 🛠️ Practice

### Project 1: Custom C++ Op
Write a Custom C++ Extension for PyTorch.
*   **Goal**: Implement a "Shift" operation that shifts a tensor by 1 unit. Bind it to Python using `torch.utils.cpp_extension`.

### Project 2: C++ Inference App
Export a ResNet model from PyTorch to TorchScript (`model.pt`).
*   **Goal**: Load this model in a C++ application using LibTorch and run inference on an image.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: How do smart pointers (`std::shared_ptr`, `std::unique_ptr`) help in AI systems?
*   **A**: They manage memory automatically. In a Graph (like a Neural Net), nodes might share ownership of weights. `shared_ptr` ensures weights are only freed when no layer uses them anymore.
*   **Q**: What is the difference between Row-Major and Column-Major memory layout?
*   **A**: It determines how a matrix is stored in linear RAM. C++ (and PyTorch/NumPy) is typically Row-Major. accessing elements in a column-wise loop ($A[1][1], A[2][1]...$) causes cache misses in Row-Major layout.
