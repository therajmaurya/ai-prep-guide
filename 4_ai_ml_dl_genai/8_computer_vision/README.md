---
title: "Computer Vision"
category: must
levels: ["mid", "senior", "principal"]
skills: [cnn, transformers, object-detection, segmentation, self-supervised]
questions:
  "mid": ["How does Max Pooling provide translation invariance?"]
  "senior": ["Compare YOLO vs Faster R-CNN architectures."]
  "principal": ["Explain how MAE (Masked Autoencoders) enables scalable self-supervised learning for vision."]
---

# Computer Vision

## 💡 Core Ideas

Computer Vision (CV) has transitioned from handcrafted features (SIFT/HOG) to CNNs (ResNet) to Transformers (ViT).

*   **Architectures**:
    *   **CNNs**: ResNet, EfficientNet (compound scaling), ConvNeXt (modernizing CNNs).
    *   **Vision Transformers (ViT)**: Patchify image -> Flatten -> Transformer Encoder. Less inductive bias than CNNs, needs more data.
*   **Tasks**:
    *   **Object Detection**:
        *   **One-Stage**: YOLO (Grid-based, fast).
        *   **Two-Stage**: Faster R-CNN (Region Proposal Network + Classifier).
    *   **Segmentation**:
        *   **Semantic**: Classify every pixel (U-Net).
        *   **Instance**: Distinguish individual objects (Mask R-CNN).
        *   **Panoptic**: Combine both.
*   **Self-Supervised Learning**:
    *   **Contrastive**: SimCLR, MoCo (Push apart different images, pull together augmentations of same image).
    *   **Masked Image Modeling**: MAE (Mask 75% of patches, reconstruct pixels).

### 2. Multimodal Vision (VLM)
*   **CLIP (Contrastive Language-Image Pre-training)**: Aligns image and text embeddings. Enables Zero-Shot classification ("A photo of a dog").
*   **LLaVA (Large Language and Vision Assistant)**: Connects a Vision Encoder (CLIP/ViT) to an LLM (Llama) via a projection layer.
*   **Flamingo**: Interleaved text and image ingestion.

### 3. Foundation Models
*   **SAM (Segment Anything Model)**: Promptable segmentation (Click to segment). Trained on 1B masks.
*   **DINOv2**: Self-supervised features that work for depth, segmentation, and retrieval out of the box.

## 📚 Resources

*   [**CS231n: Convolutional Neural Networks**](http://cs231n.stanford.edu/) - The legendary Stanford course.
*   [**Papers with Code: State of the Art**](https://paperswithcode.com/area/computer-vision) - Keep up with SOTA.
*   [**Albumentations**](https://albumentations.ai/) - The best library for image augmentation.

## 🛠️ Practice

### Project 1: Custom Object Detector
Train a **YOLOv8** model to detect custom objects (e.g., "Potholes") on a small labeled dataset (use Roboflow to label).
*   **Goal**: Deploy it to a webcam stream with OpenCV.

### Project 2: Image Segmentation with U-Net
Implement **U-Net** from scratch in PyTorch to segment medical images (or satellite, or cells).
*   **Goal**: Understand the importance of Skip Connections for localization.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is the Receptive Field of a CNN?
*   **A**: The region of the input image that a particular feature allows the neuron to "see". Deep layers have large receptive fields (global context), shallow layers have small ones (edges/textures). Dilated convolutions increase receptive field without losing resolution.

### Senior-Level
*   **Q**: Explain Non-Maximum Suppression (NMS) in Object Detection.
*   **A**: Object detectors often predict multiple bounding boxes for the same object. NMS removes redundancy:
    1.  Sort boxes by confidence score.
    2.  Pick the highest confidence box.
    3.  Discard all other boxes that have high IoU (Intersection over Union) with the picked box.
    4.  Repeat.

### Principal-Level
*   **Q**: Vision Transformers (ViT) have `O(N^2)` complexity with respect to image resolution (due to attention). How do we handle high-resolution images efficiently?
*   **A**:
    1.  **Swin Transformer**: Hierarchical attention. Compute attention only within local windows, then shift windows to allow cross-window connection. Complexity becomes linear `O(N)`.
    2.  **Pyramid Vision Transformer**: Reduces spatial resolution in stages (like a CNN).
    3.  **FlashAttention**: Hardware-aware IO optimization strictly speeds up the attention computation.
