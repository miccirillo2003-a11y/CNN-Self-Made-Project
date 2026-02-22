# CNN-Self-Made-Project

This project is a hands-on exploration of convolutional neural networks on the CIFAR-100 dataset. It compares a deep CNN built entirely from scratch with **DenseNet201**, highlighting how architectural choices affect learning, feature propagation, and overall performance.

---

## Project Overview

I implemented **PlainCNN50**, a 50-layer convolutional network with no residual or skip connections, entirely from scratch. This model follows a purely sequential structure, gradually increasing the number of channels from 32 to 512, with each layer followed by Batch Normalization and ReLU activation.  

For comparison, I also trained **DenseNet201**, a modern architecture with dense skip connections that enable feature reuse and improved gradient flow.  

The goal of this project is to understand — through building and experimentation — how different architectural designs influence training stability, performance, and learning dynamics in deep neural networks.

---

## Motivation

This project isn’t about achieving state-of-the-art results. Instead, it’s about **learning by doing**.  

Building a deep CNN from scratch allowed me to explore key challenges in deep learning, including:

- Vanishing gradients  
- How information propagates through very deep networks  
- Parameter efficiency  
- The role of skip connections in optimization  

Comparing PlainCNN50 with DenseNet201 provided a clear perspective on how modern design innovations help overcome the limitations of traditional sequential architectures.

---

## Implementation Details

**PlainCNN50**  
- 50 sequential convolutional layers  
- No residual or skip connections  
- Each layer followed by BatchNorm + ReLU  
- Channels increase from 32 → 512  

**DenseNet201**  
- Standard DenseNet201 architecture  
- Growth rate: *k = 32*  
- Dense skip connections for feature reuse  

Both models were trained on CIFAR-100 (50,000 training images, 100 classes) using data augmentation, including random flips and crops.

---

## Results & Observations

DenseNet201 consistently outperforms the plain CNN across all metrics.  

The experiment highlights the impact of architectural innovations:

- Skip connections stabilize training and mitigate vanishing gradients  
- Feature reuse improves learning efficiency  
- Deep sequential networks struggle without residual pathways  

This shows that **depth alone isn’t enough** — how layers communicate is crucial for effective training of deep networks.

---

## Personal Note

This repository represents my hands-on journey into understanding convolutional neural networks beyond theory.

PlainCNN50 was built from scratch to move away from treating deep learning models as black boxes and instead understand how design decisions affect training dynamics.

The comparison with DenseNet201 allowed me to directly observe how architectural innovations fundamentally change network behavior.

In many ways, this project acts as a learning diary — documenting exploration, experimentation, and curiosity-driven study.
