---
title: "iOS App Development"
category: good-to-know
levels: ["mid"]
skills: [ios, swift, swiftui]
questions:
  "mid": ["What is ARC (Automatic Reference Counting)?"]
  "senior": ["Design a scalable navigation architecture in SwiftUI."]
---

# iOS App Development

*   **Core ML**: Apple's framework for integrating ML models.
    *   Converts PyTorch/TF models to `.mlpackage`.
    *   Optimizes for Neural Engine (ANE) automatically.
*   **Create ML**: Tool to train simple models (Image Classifier, Tabular) directly on Mac without code.
*   **Metal Performance Shaders (MPS)**: Low-level API to use the GPU for custom matrix multiplication if Core ML doesn't support an op.
*   **Swift Concurrency**: `async/await` is critical for not blocking the main thread during heavy inference.
*   **SwiftUI vs UIKit**: SwiftUI is modern, declarative, and easier to learn. UIKit is more flexible but requires more boilerplate.

## Resources
*   [Apple Machine Learning Documentation](https://developer.apple.com/machine-learning/)
*   [Core ML Tools (Python converters)](https://apple.github.io/coremltools/)
*   [Hugging Face Swift Source](https://github.com/huggingface/swift-coreml-transformers)

## Practice
- Build a To-Do list app using SwiftUI.
- Integrate Core ML model into an iOS app.

## Questions
### Mid Level
- What is a Protocol?

### Senior Level
### Senior Level
*   **Q**: How do you optimize Core ML models for size?
*   **A**: Quantization (Float16 is standard on iOS, Int8 for weights). Breaking large models into chunks (Pipeline).
*   **Q**: Difference between Structs and Classes in Swift?
*   **A**: **Structs** are value types (copied when passed), stored in Stack (faster), no inheritance. **Classes** are reference types, stored in Heap, support inheritance. SwiftUI relies heavily on Structs for Views to be lightweight.
*   **Q**: How to run Stable Diffusion on iOS?
*   **A**: Use the `Apple/ml-stable-diffusion` repo. It utilizes the Neural Engine for the UNet and the GPU/CPU for decoding. Requires optimization of the Attention layers (Split-Einsum).
