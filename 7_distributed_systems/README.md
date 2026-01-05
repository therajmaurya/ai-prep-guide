---
title: "Distributed Systems for AI"
category: must
levels: ["senior", "principal"]
skills: [ddp, fsdp, parameter-servers, all-reduce, collective-communication]
questions:
  "mid": ["(Topic often starts at Senior) Explain Data Parallelism vs. Model Parallelism."]
  "senior": ["How does the Ring All-Reduce algorithm work? What is its time complexity?"]
  "principal": ["Architect a training cluster for a 1 Trillion parameter model. Discuss network topology, 3D Parallelism, and failure recovery."]
---

# Distributed Systems for AI

## 💡 Core Ideas

Training modern LLMs on a single GPU is impossible. Distributed training is the norm.

*   **Communication Primitives**: Broadcast, Gather, Scatter, Reduce, All-Reduce, All-Gather.
*   **Parallelism Strategies**:
    *   **Data Parallel (DDP)**: Replicate model, split data. Synchronize gradients.
    *   **Model Parallel (Tensor/Pipeline)**: Split model options across GPUs (Tensor Parallel) or split layers (Pipeline Parallel).
    *   **Zero Redundancy (ZeRO/FSDP)**: Shard optimizer states, gradients, and parameters across GPUs to save memory.
        *   **Stage 1**: Shard Optimizer States (4x memory reduction).
        *   **Stage 2**: Shard Gradients (8x memory reduction).
        *   **Stage 3**: Shard Parameters (Linear memory reduction with N GPUs). Allows fitting MASSIVE models.
        *   **Offload**: CPU Offload (move data to RAM) to fit even bigger models.
*   **Hardware**: NVLink, InfiniBand, RDMA (Remote Direct Memory Access) to bypass CPU.
*   **Consistency Models**: Synchronous (SGD) vs Asynchronous (Parameter Server).

## Applied
- Batch Processing (Hadoop, Spark).
- Stream Processing (Flink, Kafka Streams).
- Distributed Training (NCCL, MPI).
- Resource Management (YARN, Mesos).

## 📚 Resources

*   [**PyTorch Distributed Overview**](https://pytorch.org/tutorials/beginner/dist_overview.html) - Official doc.
*   [**ZeRO Paper**](https://arxiv.org/abs/1910.02054) - DeepSpeed paper explaining memory optimization.
*   [**Efficient Large-Scale Language Model Training on GPU Clusters**](https://arxiv.org/abs/2104.04473) - Practical insights from NVIDIA.
*   [Designing Data-Intensive Applications](https://dataintensive.net/)


## 🛠️ Practice

### Project 1: Multi-GPU Training
Adapt a PyTorch training script to use `DistributedDataParallel` (DDP).
*   **Goal**: Run it on a machine with 2+ GPUs (or use Colab/Kaggle T4 x2) and verify speedup is linear.

### Project 2: Implement All-Reduce
Simulate `All-Reduce` using Python multiprocessing queues or sockets.
*   **Goal**: Understand logically how gradients are summed across "nodes".

### Project 3: Spark Word Count
Write a Spark job to count word frequencies in a large dataset.

### Project 4: Flink Streaming
Set up a Flink cluster and run a streaming job.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is an RDD in Spark?
*   **A**: RDD (Resilient Distributed Dataset) is a fault-tolerant collection of elements distributed across a cluster.
*   **Q**: What happens to the Batch Size in Distributed Data Parallel?
*   **A**: The *effective* global batch size = per_gpu_batch_size * num_gpus. You must scale the learning rate accordingly (often linearly).

### Senior-Level
*   **Q**: Explain 3D Parallelism.
*   **A**: It combines Data Parallelism (split data), Tensor Parallelism (split operations within a layer), and Pipeline Parallelism (split layers across devices) to maximize utilization for massive models.

### Principal-Level
*   **Q**: A node fails during a 3-month training run. How do you ensure fast recovery without restarting from scratch?
*   **A**:
    1.  **Frequent Checkpointing**: Save state (weights + optimizer) to S3/Lustre every N steps.
    2.  **Elastic Training**: Use frameworks like `torchrun` that can detect failure, restart workers, and re-load the latest checkpoint automatically.
    3.  **Straggler Handling**: Detect slow nodes and kill/replace them.
*   **Q**: Discuss the challenges of exactly-once processing semantics.
*   **A**: Challenges include state consistency, checkpointing, and handling stragglers.
