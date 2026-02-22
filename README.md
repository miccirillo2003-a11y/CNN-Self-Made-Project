# CNN Self-Made Project

## Overview

This project is a hands-on exploration of convolutional neural networks on the **CIFAR-100** dataset. It compares a deep CNN built entirely from scratch with **DenseNet201**, highlighting how architectural choices affect learning, feature propagation, and overall performance.

The goal is not to achieve state-of-the-art results — it is about **learning by doing**: understanding why a plain sequential network of 50 layers behaves differently from a densely connected one, and what that difference teaches about gradient flow and feature reuse in practice.

---

## Motivation

Building PlainCNN50 from scratch allowed me to explore key challenges in deep learning:

- Vanishing gradients in very deep sequential networks
- How information propagates (or fails to) through depth alone
- The role of skip connections in training stability and optimization
- Parameter efficiency and its effect on generalisation

Comparing PlainCNN50 with DenseNet201 provided a direct, measurable perspective on how modern architectural innovations overcome the limitations of traditional sequential designs.

---

## Dataset: CIFAR-100

| Property | Value |
|----------|-------|
| Classes | 100 (fine-grained) |
| Training images | 50,000 |
| Test images | 10,000 |
| Image size | 32×32×3 pixels |
| Challenge | High intra-class similarity |

**Data Augmentation:** Random Horizontal Flip, Random Crop (32×32), Normalization [0,1]

> CIFAR-100 is significantly more challenging than CIFAR-10, requiring more sophisticated architectures to handle fine-grained class distinctions.

---

## Architecture Comparison

### PlainCNN50 (built from scratch)

A 50-layer convolutional network with no residual or skip connections — purely sequential depth.

| Component | Details |
|-----------|---------|
| Input | 32×32×3 |
| Conv layers | 50 × (3×3 kernel + BatchNorm + ReLU) |
| Channel growth | 32 → 512 |
| Pooling | Global Average Pooling |
| Output | FC Layer → 100 classes |

**Key limitation:** No skip connections → vanishing gradient problem. In a purely sequential network, the gradient signal must travel back through all 50 layers at every step. With no shortcuts, the chain rule progressively shrinks the gradient magnitude, causing early layers to receive near-zero updates — visible as saw-tooth oscillations in the training curves between epochs 15 and 30.

### DenseNet201

A well-known reference architecture in which every layer receives the concatenated feature maps of **all** previous layers, not just the one immediately before it.

| Component | Details |
|-----------|---------|
| Architecture | Dense blocks + Transition layers |
| Growth rate | k = 32 |
| Total layers | 201 |
| Transition layers | BatchNorm + 1×1 Conv + 2×2 Avg Pooling |

**Key advantage:** Skip connections give every layer a direct path to the loss function. No matter how deep the network, the shortest gradient path to any layer is always length 1.

### Why Skip Connections Matter

- **Solve vanishing gradient:** Gradients can flow directly backward through shortcuts
- **Feature reuse:** Low-level features remain accessible throughout the entire network
- **Enable deeper networks:** 100+ layer networks can be trained effectively
- **Implicit regularisation:** Each layer is forced to add novel features, naturally limiting memorisation

---

## Training Configuration

| Hyperparameter | Value |
|----------------|-------|
| Batch Size | 128 |
| Epochs | 50 |
| Optimizer | Adam |
| Learning Rate | 0.001 |
| Loss Function | CrossEntropyLoss |
| Hardware | GPU (CUDA) |
| Framework | PyTorch |
| Libraries | torchvision, torchmetrics, sklearn |

**Evaluation Metrics:** Training & Test Loss, Training & Test Accuracy, F1 Score (weighted), EER (Equal Error Rate)

---

## Results

### Final Metrics

| Model | Accuracy | Loss | F1 Score | EER |
|-------|----------|------|----------|-----|
| PlainCNN50 | 81.78% | 1.323 | 0.785 | 0.1421 |
| **DenseNet201** | **90.55%** | **0.830** | **0.869** | **0.0674** |
| Improvement | +8.76 pp | −37.2% | +0.084 | −52.6% |

DenseNet201 outperforms PlainCNN50 across **all metrics, in every epoch, without exception**. This consistency confirms that the performance difference is structural, not due to luck or hyperparameter tuning.

---

### Loss: Convergence and Stability

DenseNet201 descends faster right from epoch 1, reaching a training loss of ~0.35 by epoch 50 vs. PlainCNN50's ~0.86 — more than twice as high. On the test set, DenseNet201 stabilises at **0.830** vs. PlainCNN50's **1.323** (−37.2%).

The blue dashed curve (PlainCNN50 test loss) shows visible oscillations between roughly epochs 15 and 30. DenseNet201's curves remain consistently smooth throughout — a direct consequence of the dense gradient pathways.

---

### Accuracy: Learning Speed and Generalisation

DenseNet201 leads from the very first epoch and never falls behind:

| | PlainCNN50 | DenseNet201 |
|---|---|---|
| Test Accuracy (epoch 50) | 81.78% | **90.55%** |
| Train Accuracy (epoch 50) | ~91% | ~93% |
| Train–Test Gap | ~9 pp | ~2.5 pp |

The train–test gap is arguably more informative than absolute accuracy. PlainCNN50's training accuracy approaches DenseNet201's (~91–93%), but its test accuracy lags by almost 9 percentage points — a classic overfitting signature. The network has learned to reproduce training examples rather than extract generalisable patterns.

> On CIFAR-100, a 9 pp test accuracy gap translates to roughly **876 more correct classifications per 10,000 images**. For a 100-class problem where random chance is 1%, this is a substantial real-world difference.

---

### F1 Score: Balanced Performance Across All 100 Classes

The weighted F1 score weights each class by its size, reflecting performance across all 100 categories rather than hiding imbalances behind average accuracy.

| | PlainCNN50 | DenseNet201 |
|---|---|---|
| F1 at epoch 1 | 0.14 | 0.27 |
| F1 at epoch 50 | 0.785 | **0.869** |
| Gap | — | +0.084 |

The F1 gap (~10.7% relative) is proportionally consistent with the accuracy gap (+8.76 pp, ~10.7% relative), confirming that DenseNet201's superiority holds **across all 100 classes**, not just a few dominant categories.

---

### EER: Reliability of the Classifier

The Equal Error Rate (EER) is the point on the ROC curve where the False Acceptance Rate equals the False Rejection Rate. A lower EER is better: 0 = perfect separation, 0.5 = random chance.

| | PlainCNN50 | DenseNet201 |
|---|---|---|
| EER at epoch 50 | 0.1421 | **0.0674** |
| Reduction | — | −52.6% |

By epoch 10, DenseNet201 already achieves EER values that PlainCNN50 only matches around epochs 35–40. At the end of training, DenseNet201 makes roughly **half the threshold-level errors** of PlainCNN50.

---

## Analysis: Why DenseNet201 Wins

**DenseNet201 advantages:**
- Skip connections → improved gradient flow
- Feature reuse → preserved information across layers
- Efficient parameters → less overfitting
- Concatenation → implicit regularisation (acts like dropout)

**PlainCNN50 limitations:**
- Vanishing gradient in deep sequential networks
- Information lost layer by layer
- More parameters but less effective use of them
- Slower convergence with visible oscillations

---

## When to Use Each Architecture

| Use DenseNet when... | Use PlainCNN when... |
|----------------------|----------------------|
| Complex tasks (100+ classes) | Simple classification tasks |
| Limited compute budget | Few layers are sufficient |
| Fast convergence is needed | Interpretability is a priority |
| Generalisation is critical | Prototyping quickly |

---

## Key Takeaways

1. **Depth alone is not enough.** 50 layers is a serious network — but stacking layers sequentially without any mechanism to preserve gradient flow leads to predictable problems: slow convergence, oscillations, and overfitting.

2. **Skip connections solve a real engineering problem.** The oscillations in PlainCNN50's loss curve are not noise — they are the signature of vanishing gradients. DenseNet's dense connections eliminate this structurally.

3. **Feature reuse improves generalisation.** PlainCNN50's 9 pp train–test gap tells us the network is memorising rather than generalising. Dense connectivity forces each layer to contribute new information, acting as a natural regulariser.

4. **Architecture decisions have measurable, consistent consequences.** The performance difference is structural and visible in every metric, every epoch.

> **Next steps:** The natural follow-up would be to add skip connections to PlainCNN50 (making it a ResNet-style architecture) and re-run the comparison, to isolate exactly how much of the gap is due to dense connectivity specifically versus skip connections in general.

---

## Personal Note

This repository represents my hands-on journey into understanding convolutional neural networks beyond theory. PlainCNN50 was built from scratch to move away from treating deep learning models as black boxes and instead understand how design decisions affect training dynamics.

In many ways, this project acts as a **learning diary** — documenting exploration, experimentation, and curiosity-driven study.

---

## Author

**Michele Cirillo** — February 2026
Master's Degree in Data Science and Artificial Intelligence
`miccirillo2003@gmail.com`
