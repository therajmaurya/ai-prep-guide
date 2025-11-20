---
title: "Other Languages for AI/ML"
category: coding
levels: ["senior", "principal"]
skills: [c++, julia, memory-management, performance]
questions:
  "senior": ["Why is C++ used for DL frameworks?", "Explain Pointers vs References in C++.", "What is Multiple Dispatch in Julia?"]
  "principal": ["How to bind C++ code to Python (pybind11)?", "Discuss the trade-offs between Julia and Python for scientific computing."]
---

# Other Languages: C++ and Julia

While Python is the interface, the engine of AI is written in C++. Understanding C++ is crucial for optimizing inference engines (TensorRT, ONNX Runtime) and writing custom ops. Julia is emerging as a high-performance alternative for scientific computing.

## C++ for Machine Learning

### 1. Core Concepts
*   **Memory Management**: Manual control via `new`/`delete` (or smart pointers `std::unique_ptr`, `std::shared_ptr`). Crucial for managing GPU buffers.
*   **Pointers vs References**:
    *   **Pointer (`*`)**: Holds a memory address. Can be null. Can be reassigned.
    *   **Reference (`&`)**: Alias for an existing variable. Cannot be null. Cannot be reassigned.
*   **Move Semantics (`std::move`)**: Efficiently transfers ownership of resources (like large tensors) without copying.

### 2. Templates
*   Allows writing generic code (e.g., a matrix class that works for `float`, `double`, `int`).
*   Heavily used in PyTorch/TensorFlow codebases to support multiple data types.

### 3. Binding to Python
*   **pybind11**: The standard tool for exposing C++ classes and functions to Python.
*   **Zero-Copy**: Crucial to pass pointers (e.g., NumPy array data pointer) to C++ without copying data.

## Julia for Scientific Computing

### 1. Why Julia?
*   **"Walks like Python, runs like C"**. JIT compiled (LLVM).
*   Solves the "Two-Language Problem" (prototyping in Python, rewriting in C++).

### 2. Multiple Dispatch
*   The killer feature. Functions are defined by the *combination* of argument types.
*   Allows for extremely composable and extensible code.
    ```julia
    f(x::Int, y::Int) = "Two Ints"
    f(x::String, y::String) = "Two Strings"
    f(x, y) = "Generic"
    ```

## Interview Questions

### Senior Level
1.  **What is the difference between Stack and Heap memory in C++?**
    *   *Answer*: Stack is fast, automatic allocation/deallocation (scope-based), limited size. Heap is slower, manual allocation (`new`), large size. Memory leaks happen on the Heap.
2.  **Why do we use `const &` (const reference) in function arguments?**
    *   *Answer*: To avoid copying large objects (efficiency of pass-by-reference) while guaranteeing that the function won't modify the object (safety of pass-by-value).
3.  **Explain RAII (Resource Acquisition Is Initialization).**
    *   *Answer*: A C++ idiom where resources (memory, file handles, locks) are acquired in the constructor and released in the destructor. Ensures leak-free code even if exceptions are thrown.

### Principal Level
1.  **Compare the memory model of Python vs C++.**
    *   *Discussion*: Python: Everything is an object on the heap, managed by Ref Counting + GC. C++: Value semantics by default (objects on stack), manual heap management. C++ gives control over data layout (struct of arrays vs array of structs) for cache optimization.
2.  **How does Julia achieve performance comparable to C?**
    *   *Discussion*: Type inference and specialization. The compiler generates specialized machine code for each combination of argument types called. No interpreter overhead at runtime.
