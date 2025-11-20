---
title: "PyTorch Fundamentals"
category: coding
levels: ["mid", "senior", "principal"]
skills: [pytorch, tensors, autograd, distributed-training, torchscript]
questions:
  "mid": ["What is Autograd and how does it work?", "Difference between `nn.Module` and `nn.Sequential`.", "How to move tensors between CPU and GPU?"]
  "senior": ["Implement a custom Autograd function.", "Explain DistributedDataParallel (DDP) vs DataParallel (DP).", "How does PyTorch handle dynamic computation graphs?"]
  "principal": ["Optimize a PyTorch model for inference using TorchScript/ONNX.", "Discuss memory management in PyTorch (caching allocator).", "Design a custom DataSampler for imbalanced datasets."]
---

# PyTorch Fundamentals

PyTorch is the dominant framework for research and production AI. Its key differentiator is the **Dynamic Computation Graph (Define-by-Run)**, which allows for flexible model architectures (loops, conditions) and easier debugging compared to static graph frameworks.

## Core Concepts

### 1. Tensors
The fundamental data structure. Similar to NumPy arrays but with GPU support.
*   **Creation**: `torch.tensor()`, `torch.randn()`, `torch.zeros()`.
*   **Device**: `tensor.to('cuda')` moves data to GPU.
*   **Reshaping**: `view()` (requires contiguous memory) vs `reshape()` (handles non-contiguous).

### 2. Autograd (Automatic Differentiation)
PyTorch records operations on tensors to build a computation graph.
*   **`requires_grad=True`**: Tells PyTorch to track operations on this tensor.
*   **`backward()`**: Computes gradients using the chain rule.
*   **`grad`**: Attribute where gradients are accumulated.
*   **`no_grad()`**: Context manager to disable gradient tracking (inference mode).

```python
x = torch.tensor([2.0], requires_grad=True)
y = x ** 2
y.backward()
print(x.grad) # 4.0 (dy/dx = 2x)
```

### 3. `nn.Module`
The base class for all neural network modules.
*   **`__init__`**: Define layers/parameters.
*   **`forward`**: Define the computation (the graph).
*   **Parameters**: `list(model.parameters())` returns all learnable weights.

### 4. Dataset & DataLoader
*   **Dataset**: Stores samples and labels. Must implement `__len__` and `__getitem__`.
*   **DataLoader**: Wraps an iterable around the Dataset. Handles batching, shuffling, and **multiprocessing** (`num_workers`).

## Advanced Topics

### 1. Distributed Training
*   **DataParallel (DP)**: Single-node, multi-GPU. Replicates model on each GPU, splits data. High overhead due to GIL and scatter/gather on master GPU.
*   **DistributedDataParallel (DDP)**: Multi-node, multi-GPU. Spawns a separate process for each GPU. Each process has its own model replica and optimizer. Gradients are synchronized using `AllReduce` (Ring-AllReduce). **Preferred over DP**.

### 2. TorchScript & JIT
*   **Tracing**: `torch.jit.trace(model, input)`. Records operations by running the model. Fails with dynamic control flow (if/else).
*   **Scripting**: `torch.jit.script(model)`. Compiles the model code directly. Supports control flow.
*   **Benefit**: Decouples model from Python interpreter, allowing execution in C++ (LibTorch) for high-performance inference.

### 3. Custom Autograd Functions
Extend `torch.autograd.Function` to define custom forward and backward passes. Useful for non-differentiable operations or numerical stability fixes.

```python
class MyReLU(torch.autograd.Function):
    @staticmethod
    def forward(ctx, input):
        ctx.save_for_backward(input)
        return input.clamp(min=0)

    @staticmethod
    def backward(ctx, grad_output):
        input, = ctx.saved_tensors
        grad_input = grad_output.clone()
        grad_input[input < 0] = 0
        return grad_input
```

## Interview Questions

### Mid Level
1.  **What is the difference between `model.train()` and `model.eval()`?**
    *   *Answer*: `train()` enables Dropout and Batch Normalization layers to update running stats. `eval()` disables Dropout and uses fixed running stats for BatchNorm.
2.  **Why do we zero out gradients (`optimizer.zero_grad()`) before the backward pass?**
    *   *Answer*: PyTorch *accumulates* gradients in the `.grad` attribute by default (useful for RNNs). If not zeroed, gradients from the previous batch would be added to the current one.
3.  **Explain the difference between `view` and `reshape`.**
    *   *Answer*: `view` returns a new tensor with the same data but different shape; it requires the data to be contiguous in memory. `reshape` works on non-contiguous data too (it may copy the data).

### Senior Level
1.  **How does DistributedDataParallel (DDP) work internally?**
    *   *Answer*: It uses a multi-process architecture. Each GPU runs its own process with a model replica. During backward pass, gradients are synchronized across processes using the `AllReduce` collective communication primitive (often Ring-AllReduce) to ensure all models stay in sync.
2.  **What is the "reparameterization trick" in VAEs and how is it implemented in PyTorch?**
    *   *Answer*: We cannot backpropagate through a random sampling node. The trick expresses the random variable $z$ as $\mu + \sigma \cdot \epsilon$, where $\epsilon \sim N(0, 1)$. This moves the stochasticity to an input node, allowing gradients to flow through $\mu$ and $\sigma$.
3.  **How would you debug a "CUDA Out of Memory" error?**
    *   *Answer*: Check batch size, check for accumulated history (e.g., `total_loss += loss` keeps the graph alive; use `loss.item()`), use `torch.cuda.empty_cache()`, or use gradient checkpointing (`torch.utils.checkpoint`) to trade compute for memory.

### Principal Level
1.  **Design a training pipeline for a trillion-parameter model.**
    *   *Discussion*: Standard DDP is not enough (model won't fit in GPU RAM). Need **Model Parallelism** (Tensor Parallelism like Megatron-LM) or **Pipeline Parallelism**. Also use **ZeRO (Zero Redundancy Optimizer)** to shard optimizer states and gradients across GPUs (DeepSpeed/FSDP).
2.  **Explain the PyTorch Caching Allocator.**
    *   *Discussion*: PyTorch uses a custom memory allocator (based on `malloc`) to manage CUDA memory. It caches blocks of memory to avoid expensive `cudaMalloc` calls. This can lead to fragmentation. `nvidia-smi` shows reserved memory, not actual usage.
3.  **How to implement a custom operator in C++/CUDA and bind it to PyTorch?**
    *   *Discussion*: Write the kernel in CUDA, write the C++ wrapper using `torch::Tensor` API, and use `pybind11` or `torch.utils.cpp_extension` to compile and load it as a Python module.
