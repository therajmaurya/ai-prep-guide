---
title: "CUDA Programming"
category: coding
levels: ["senior", "principal"]
skills: [cuda, c++, gpu-architecture, parallel-computing, optimization]
questions:
  "senior": ["Explain the CUDA memory hierarchy.", "What is a Warp and Warp Divergence?", "How to optimize a matrix multiplication kernel?"]
  "principal": ["Discuss Tensor Cores and Mixed Precision training.", "How to debug a race condition in a CUDA kernel?", "Explain Unified Memory and its performance implications."]
---

# CUDA Programming for ML Engineers

While most ML engineers work with high-level frameworks (PyTorch), understanding CUDA (Compute Unified Device Architecture) is essential for optimizing kernels, writing custom operators, and debugging performance bottlenecks.

## Core Concepts

### 1. GPU Architecture
*   **SM (Streaming Multiprocessor)**: The core unit of the GPU. Contains CUDA cores, Tensor Cores, Shared Memory, and Register File.
*   **Grid, Block, Thread**:
    *   **Grid**: The collection of all threads launched for a kernel.
    *   **Block**: A group of threads that execute on the same SM. Can share memory (Shared Memory) and synchronize (`__syncthreads()`).
    *   **Thread**: The smallest unit of execution.

### 2. Memory Hierarchy (Fastest to Slowest)
1.  **Registers**: Private to each thread. Fastest. Limited size.
2.  **Shared Memory (L1)**: Shared by threads in a **Block**. User-managed cache. Crucial for optimization (tiling).
3.  **L2 Cache**: Shared by all SMs.
4.  **Global Memory (VRAM)**: The main GPU memory (e.g., 80GB on A100). Slowest. Access must be **coalesced** for performance.

### 3. Kernel Execution
*   **`__global__`**: Function called from CPU, runs on GPU.
*   **`__device__`**: Function called from GPU, runs on GPU.
*   **`__host__`**: Function called from CPU, runs on CPU.
*   **Launch Syntax**: `kernel<<<gridDim, blockDim>>>(args)`

```cpp
__global__ void add(int *a, int *b, int *c, int n) {
    int index = threadIdx.x + blockIdx.x * blockDim.x;
    if (index < n)
        c[index] = a[index] + b[index];
}
```

## Advanced Topics

### 1. Warps & Divergence
*   **Warp**: A group of 32 threads executed simultaneously (SIMT - Single Instruction Multiple Threads).
*   **Divergence**: If threads in a warp take different paths (e.g., `if-else`), the warp executes *both* paths sequentially, masking out threads that shouldn't execute. This kills performance.
    *   *Fix*: Avoid branch divergence within a warp.

### 2. Memory Coalescing
*   When threads in a warp access global memory, the hardware attempts to combine (coalesce) accesses into a single transaction.
*   *Requirement*: Consecutive threads should access consecutive memory addresses.
*   *Failure*: Strided access (e.g., accessing column-major data row-by-row) causes multiple memory transactions, reducing bandwidth.

### 3. Tensor Cores
*   Specialized hardware units (Volta+) for matrix multiplication ($D = A \times B + C$).
*   Operate on $4 \times 4$ or $16 \times 16$ matrices in one cycle.
*   Crucial for Mixed Precision Training (FP16/BF16).

### 4. Streams & Events
*   **Stream**: A sequence of operations that execute in order.
*   **Concurrency**: Operations in *different* streams can run concurrently (e.g., copy data while computing).
*   **Events**: Synchronization points to measure time or coordinate streams.

### 5. Profiling & Optimization Metrics
*   **Occupancy**: The ratio of active warps to maximum supported warps on an SM. Higher occupancy hides memory latency (one warp waits, another executes).
*   **NVTX (NVIDIA Tools Extension)**: Annotate your code with ranges and markers to see them in Nsight Systems profiler. Useful to correlate CPU PyTorch calls with GPU kernels.

## Interview Questions

### Senior Level
1.  **What is Shared Memory and why is it important?**
    *   *Answer*: It's on-chip memory (L1 cache) explicitly managed by the programmer. It is much faster than Global Memory. It allows threads in a block to cooperate and reuse data (e.g., loading a tile of a matrix once and using it multiple times), significantly reducing global memory bandwidth pressure.
2.  **Explain "Bank Conflicts" in Shared Memory.**
    *   *Answer*: Shared memory is divided into banks (usually 32). If multiple threads in a warp access different addresses that map to the *same* bank, the accesses are serialized (bank conflict).
3.  **How do you synchronize threads in CUDA?**
    *   *Answer*: `__syncthreads()` synchronizes threads within a **block**. To synchronize across the entire grid, you usually finish the kernel launch (implicit sync) or use Cooperative Groups (advanced).

### Principal Level
1.  **How would you optimize a reduction kernel (e.g., sum of an array)?**
    *   *Discussion*:
        *   Use Shared Memory to reduce within a block first.
        *   Use tree-based reduction ($O(\log N)$ steps).
        *   Avoid warp divergence (keep active threads contiguous).
        *   Unroll the last few iterations (warp reduction).
        *   Use Warp Shuffle functions (`__shfl_down_sync`) to exchange data between threads without shared memory (faster).
2.  **Discuss the challenges of training Large Language Models (LLMs) on GPUs.**
    *   *Discussion*: Memory capacity (VRAM) is the bottleneck. Need techniques like:
        *   **Activation Checkpointing**: Recompute activations during backward pass to save memory.
        *   **Kernel Fusion**: Fuse operations (e.g., Scale + Mask + Softmax) to reduce memory I/O (FlashAttention).
        *   **Quantization**: FP8/INT8 training.
3.  **Explain the difference between Pinned (Page-Locked) Memory and Pageable Memory.**
    *   *Discussion*: Pinned memory is allocated in host RAM but cannot be swapped out by the OS. It allows for faster PCIe transfers (DMA) and is required for asynchronous copies (`cudaMemcpyAsync`). Pageable memory is standard OS memory.
