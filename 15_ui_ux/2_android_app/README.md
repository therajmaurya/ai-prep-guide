---
title: "On-Device AI (Android)"
category: ui-ux
levels: ["senior"]
skills: [tflite, execuTorch, quantization, onnx-runtime]
questions:
  "mid": ["What is TFLite?"]
  "senior": ["How do you optimize a PyTorch model for mobile deployment?"]
  "principal": ["Design a privacy-preserving 'Photos' app that indexes faces 100% offline."]
---

# On-Device AI (Android)

## 💡 Core Ideas

Running AI on the Edge (Phone) saves server costs and protects privacy.

*   **Frameworks**:
    *   **TensorFlow Lite (TFLite)**: The legacy standard.
    *   **ONNX Runtime**: Cross-platform.
    *   **ExecuTorch**: PyTorch's modern edge solution.
*   **Optimization**:
    *   **Quantization**: FP32 -> Int8. 4x smaller, faster on DSP/NPU.
    *   **Pruning**: Removing connections.
*   **NPU (Neural Processing Unit)**: Accelerators on Snapdragon/Apple chips.

## Components:
- Activities and Fragments.
- Jetpack Compose.
- Coroutines for async programming.
- MVVM Architecture.

## 🛠️ Practice

### Project 1: Offline Object Detector
Deploy a YOLOv8-nano model to an Android App using TFLite.
*   **Goal**: Draw bounding boxes on the Camera2 API preview stream at 30 FPS.

*   **Project 2: RAG on Android**
    *   Use **MediaPipe LLM Inference** or **ExecuTorch** to run a small LLM (e.g., Gemma 2B or Llama 3 1B) on-device.
    *   Store embeddings in a local **SQLite** database (with vector extension if available, or manual cosine similarity).
*   **Project 3: Audio classifier**
    *   Record audio using `AudioRecord` and classification using TFLite Audio task.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: What is Post-Training Quantization (PTQ) vs Quantization Aware Training (QAT)?
*   **A**:
    *   **PTQ**: Convert a pre-trained FP32 model to Int8. Fast/Easy, slight accuracy drop.
    *   **QAT**: Simulate quantization noise *during* training. Harder workflow, but recovers the accuracy loss (Essential for 4-bit models).

*   **Q**: Explain Neural Networks API (NNAPI) on Android.
*   **A**: It's a C-API designed for running computationally intensive ops for machine learning on Android devices. It abstracts the hardware (GPU, DSP, NPU) so frameworks like TFLite can run efficiently on any device.
*   **Q**: How do you handle large model files (e.g., 500MB) in an APK?
*   **A**: Don't bundle them in `assets` if possible (bloats download size). Download them on first launch using `WorkManager` or `DownloadManager`, or use **Play Asset Delivery** (install-time or on-demand delivery).
*   **Q**: Difference between Fragment and Activity in context of Memory Leaks with AI models?
*   **A**: AI Interpreters (like TFLite Interpreter) are heavy. If you initialize them in a Fragment and don't close them in `onDestroy`, recreating the fragment (rotate screen) will leak the memory, leading to OOM. Use a `ViewModel` to hold the Interpreter instance or a Singleton repository.
