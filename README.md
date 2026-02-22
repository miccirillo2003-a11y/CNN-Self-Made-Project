# CNN-Self-Made-Project

**Project Overview**

This project provides a controlled comparison between two convolutional neural network architectures trained on the CIFAR-100 dataset:

PlainCNN50 — a 50-layer convolutional neural network implemented entirely from scratch, without residual or skip connections

DenseNet201 — a modern architecture based on dense skip connections and feature reuse

The main objective is to understand, through direct implementation and experimentation, how architectural design choices influence training stability and model performance in deep neural networks.

**Motivation**

Rather than pursuing state-of-the-art performance, this project focuses on learning by building.

Implementing a deep CNN from scratch allowed me to explore fundamental challenges in deep learning, such as:

vanishing gradients

information propagation across deep networks

parameter efficiency

the impact of skip connections on optimization

DenseNet201 serves as a strong reference architecture to better understand how modern design principles overcome the limitations of deep sequential models.

**Implementation Details**
-PlainCNN50

50 sequential convolutional layers

No residual or skip connections

Each layer followed by:

Batch Normalization

ReLU activation

Channel depth progressively increases from 32 → 512

-DenseNet201

Standard DenseNet201 architecture

Growth rate: k = 32

Dense skip connections enabling feature reuse


**Results & Observations**

DenseNet201 consistently outperforms the plain CNN across evaluation metrics.

The observed performance gap highlights how:

skip connections help mitigate vanishing gradients,

feature reuse improves learning efficiency,

deep sequential networks struggle without residual pathways.

This experiment provides a practical demonstration of why modern CNN architectures rely heavily on connectivity patterns rather than simply increasing depth.

**Personal Note**

This repository represents my hands-on journey into understanding convolutional neural networks beyond theory.

PlainCNN50 was built from scratch to move away from treating deep learning models as black boxes and instead understand how design decisions affect training dynamics.

The comparison with DenseNet201 allowed me to directly observe how architectural innovations fundamentally change network behavior.

In many ways, this project acts as a learning diary — documenting exploration, experimentation, and curiosity-driven study.
