---
layout:     post
title:      MobileNets:open-source Models for efficient on-device vision
subtitle:   Mobilenet network
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - CNN
 - Deep Learning
 - Neural Network
---

Visual reognition for on device and embedded applications poses mnay challenges: 
 - model must run quickly with high accuracy
 - resource-constrained environment

THis paper describes an efficient network and **a set of two hyper-parameters** in order to build very samll, low latency models.

### previous work:
 - Depthwais seperable convolutions
 - Flattened networks.
 - Factorized networks
 - Xception network
 - Squeezenet
 - different approach: Shrinking
 - Another method for training small network: 
    - distillation: which use a larger network to teach a small network, this is complementary to mobilenet and is covered in section 4.
### mobilenet architecture

> seperable convolution :https://towardsdatascience.com/a-basic-introduction-to-separable-convolutions-b99ec3102728

> why does spatial seperable convolution reduce multiplication?


#### traditional convolution:
**input**: $D_F * D_F * M$   **output**: $D_G * D_G *N$
**kernel**:$D_k*D_k*M*N$
*computational cost*: $D_K*D_K*M*N*D_F*D_F$

<img src="https://i.ibb.co/tx3xCPF/mobilenet.png" alt="mobilenet" border="0"></a>


### width multiplier:
$\alpha$   channels: $\alpha M$, $\alpha N$
### Resolution multiplier:

<br>
<hr>





## MObileNet_v2: inverted Residuals and Linear bottleneck.

### inverted residuals:
 - original residual blocks: wide-narrow-wide
<img src="https://miro.medium.com/max/1184/1*5Jdh_PDTXp0uhF8c79TEsQ.png">
 - inverted: narrow - wide- narrow
 <img src="https://miro.medium.com/max/765/1*BaxdP8RS5x_EVMNJSd1Urg.png">


 ### linear bottleneck

 remove the last activation layer before the last convolutional layer.


 ### relu 6 instead of relu