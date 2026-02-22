# CNN-Self-Made-Project

  **ABSTRACT**

This study presents a controlled comparison between two convolutional neural network architectures on the CIFAR-100 dataset: a plain 50-layer CNN implemented from scratch without residual connections, and DenseNet201, which employs dense skip connections and feature reuse. Both models were trained under identical conditions using the Adam optimizer for 50 epochs. Results demonstrate that DenseNet201 significantly outperforms the plain architecture across all metrics. The performance gap is attributed to DenseNet's skip connections, which mitigate vanishing gradients and enable effective feature reuse throughout the network. This experiment quantifies the tangible benefits of modern architectural innovations and highlights the limitations of deep sequential networks without residual pathways.

**IMPLEMENTATION**

PlainCNN50 was implemented from scratch with 50 sequential convolutional layers, each followed by BatchNorm and ReLU, with channels growing from 32 to 512. DenseNet201 was implemented using the standard architecture with growth rate k=32. Both models were trained on CIFAR-100 (50K training images, 100 classes) with data augmentation including random flips and crops.

**PERSONAL NOTE**

This project was never intended to break new ground or achieve state-of-the-art results. Its purpose is far simpler and more personal: it represents my hands-on journey into the inner workings of convolutional neural networks. I built PlainCNN50 from scratch not because the world needs another CNN implementation, but because I wanted to truly understand what happens inside these architectures that we so often treat as black boxes.The comparison with DenseNet201 was the perfect counterpoint, allowing me to see firsthand how architectural innovations like skip connections fundamentally change network behavior. This repository is essentially my learning diary, documenting what I discovered about vanishing gradients, feature reuse, parameter efficiency, and the subtle ways that information propagates through deep networks and the primary goal was my own education and curiosity.
