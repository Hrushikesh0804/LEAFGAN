# LeafGAN: An Effective Data Augmentation Method for Practical Plant Disease Diagnosis

LeafGAN is a GAN-based image-to-image translation framework designed to improve the robustness and generalization of automated plant disease diagnosis systems. Unlike vanilla CycleGAN, LeafGAN focuses exclusively on transforming the **leaf regions** while preserving real-world backgrounds, resulting in more realistic synthesized disease images and significantly enhanced classifier performance on unseen environments.

This repository summarizes the concepts, methodology, experiments, and findings based on the LeafGAN research.

---

## Overview

Plant disease diagnosis models often suffer from overfitting due to:
- Limited disease images  
- Imbalanced datasets  
- Lack of background diversity  
- Small and subtle disease features  
- Bias introduced by laboratory-like image settings  

LeafGAN addresses these issues by generating realistic diseased leaf images from healthy ones. The method integrates a novel label-free segmentation module (LFLSeg) to detect leaf regions and guide the GAN to transform only the relevant pixels. This results in highly natural disease augmentations that improve classifier robustness.

---

## Key Features

### Leaf-Focused Translation
CycleGAN-type models tend to alter both the leaf and the background. LeafGAN fixes this with:
- Mask-guided adversarial training  
- Background-similarity loss  
- Weakly supervised segmentation masks  

### Label-Free Leaf Segmentation (LFLSeg)
LFLSeg is built on a fine-tuned ResNet-101 classifier that distinguishes:
- Full leaf  
- Partial leaf  
- Non-leaf  

Using Grad-CAM, it generates segmentation masks without requiring pixel-level labels.

### Improved Diagnostic Performance
Testing on an unseen dataset showed:
- Baseline accuracy: **71.3 percent**
- Baseline + CycleGAN augmentation: **72.0 percent**
- Baseline + LeafGAN augmentation: **78.7 percent**

This demonstrates a **7.4 percent improvement** in generalization using LeafGAN-generated images.

---

## Architecture

LeafGAN extends CycleGAN with three major components:

### 1. LFLSeg Module
Produces soft leaf masks using weakly supervised learning and Grad-CAM heatmaps.

### 2. LeafGAN Translation Network
Includes:
- Two generators (healthy→disease and disease→healthy)  
- Two discriminators  
- Mask-guided adversarial loss  
- Cycle-consistency loss  
- Background similarity loss  

The masks ensure that only leaf regions are transformed during training.

### 3. Disease Classification Pipeline
Three classification models are used for evaluation:
- Baseline  
- Baseline + CycleGAN-generated images  
- Baseline + LeafGAN-generated images  

All classifiers are fine-tuned from ResNet-101.

---

## Dataset

The authors collected cucumber leaf images from multiple farms in Japan (2015–2019).  
Two datasets were used:

### Dataset A (Training + Validation)
Contains healthy leaves and three disease classes:
- MYSV  
- Brown spot  
- Powdery mildew  

### Dataset B (Unseen Test Set)
Captured under different conditions to evaluate practical performance.

Dataset characteristics:
- Images contain single leaves  
- Natural, diverse backgrounds  
- Variations in lighting, color, and leaf health stage  

---

## Experimental Summary

### Segmentation Performance
- LFLSeg classification accuracy: **99.8 percent**
- F1-score for segmentation: **83.9 percent** (no pixel mask supervision)

### Image Generation Quality
LeafGAN:
- Preserves real backgrounds  
- Adds disease symptoms only on leaf regions  
- Produces highly realistic augmentations  

CycleGAN:
- Alters entire image  
- Produces less plausible disease images  

### Classification Results on Unseen Dataset B

| Model | Average Accuracy |
|-------|------------------|
| Baseline | 71.3 percent |
| Baseline + CycleGAN | 72.0 percent |
| Baseline + LeafGAN | **78.7 percent** |

---

## Limitations

While LeafGAN greatly improves data diversity, some limitations remain:
- LFLSeg struggles with images containing multiple overlapping leaves  
- Some disease types (e.g., powdery mildew) may be transformed as color changes rather than clear symptoms due to limited dataset variation  
- Scale and camera-distance variation may affect mask quality  

Future improvements may include:
- More robust segmentation models  
- Disease-specific transformation controls  
- Multi-leaf image handling  

---

## Conclusion

LeafGAN is a practical and effective data augmentation tool for plant disease diagnosis systems.  
By focusing transformations exclusively on the leaf areas and preserving diverse backgrounds, it:
- Generates realistic disease images  
- Significantly improves classifier generalization  
- Reduces dependency on expensive labeled datasets  

LeafGAN represents a strong step forward for reliable machine learning in agriculture.

---

