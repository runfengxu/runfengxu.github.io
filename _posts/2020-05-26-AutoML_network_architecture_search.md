---
layout:     post
title:      MnasNet
subtitle:   Machine learning to explore neural network architecture
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - CNN
 - Deep Learning
 - Neural Network
---
**source from google AI blog**

Designing machine learning models are painstaking because the search space of all possible models can be combinatorially large.

Among all algorithms, *evolutionary algorithm* and *reinforcement learning algorithm* have shown great promises. But this article only focus on reinforcentment learning.


In their approach, a controller neural net can propose a 'child' model architecture, which can be trained and evaluated for quality on a particular task. the feedbakc is then used to inform the controller how to improve.

<img src="https://1.bp.blogspot.com/-0nzARW3QtkA/WRtuVsUJ02I/AAAAAAAAB0s/t6ncpAH6VfIzkr2tWW8CnE6U2Es2Bs1BgCLcB/s1600/image3.png" alt="mobilenet" border="0"></a>

THey tested it on : image recognition with COFAR-10 and language modeling with Penn Treebank, both achieve stage-of-art accuracy.

### AutoML for large scale image classification and object detection

datasets like ImageNet and COCO are orders of magnitude larger than CIFAR-10 and taks many months to train.
So google researchers took a alternate approach to be more tractable to large-scale datasets.

 - redesigned the search space, and layer can be stacked many times in a flexible manner.
 - perform architecture search on CIFAR-10 and transferred the best learned architecture to ImageNet and COCO.

And the result is NasNet

### MNasnet:
1.5x faster than MobileNetV2, 2.4x faster than NASNet.

Main components:
 - RNN-based controller for learning  and sampling model architecture.
 - a trainer that builds and trains models to obtrain the accuracy.
 - An inference engine for measuring the model speed on real mobile phones using TensorFlow Lite.

 <img src="https://3.bp.blogspot.com/-AdjfrZWQ0as/W2jkUwfCZwI/AAAAAAAADNM/cedodZCGRFQaD075xxIQpe2gU9bYay3xwCLcBGAs/s640/image1.png">


 proposed A novel factorized hierarchical search space,:
  - factorize a convolutional neural network into a sequence of blocks, and then uses a hierarchical search spaces to determine the layer architecture for each block. THerefore different operations and connections are allowed in each layer, meanwhile force all layers in each block to share same structure.