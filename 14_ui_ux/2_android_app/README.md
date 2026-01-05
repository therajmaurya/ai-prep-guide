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

* Build an app that consumes a REST API.
* Implement a local database using Room.

## 🗣️ Interview Questions

### Senior-Level
*   **Q**: What is Post-Training Quantization (PTQ) vs Quantization Aware Training (QAT)?
*   **A**:
    *   **PTQ**: Convert a pre-trained FP32 model to Int8. Fast/Easy, slight accuracy drop.
    *   **QAT**: Simulate quantization noise *during* training. Harder workflow, but recovers the accuracy loss (Essential for 4-bit models).

* Discuss memory management in Android.
* Explain the difference between a Fragment and an Activity.
* What is a Service & API in Android?
* How do you handle permissions in Android?
