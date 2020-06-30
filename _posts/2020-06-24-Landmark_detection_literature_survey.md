---
layout:     post
title:      landmark detection literature survey
subtitle:   landmark detection
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - Computer Vision
 - Neural network architecture
---

> paper: 2018: https://arxiv.org/abs/1805.05563

# 2019 RetinaFace: Single-stage Dense Face Localisation in the Wild.


# 2017 RetinaNet:
https://towardsdatascience.com/review-retinanet-focal-loss-object-detection-38fba6afabe4

> 1.3. Number of Boxes Comparison
 - YOLOv1: 98 boxes
 - YOLOv2: ~1k
 - OverFeat: ~1–2k
 - SSD: ~8–26k
 - RetinaNet: ~100k. RetinaNet can have ~100k boxes with the resolve of class imbalance problem using focal loss.

 > Cross Entropy: $−(ylog(p)+(1−y)log(1−p))$ for m=2

 > α-Balanced CE Loss <img src='https://miro.medium.com/max/185/1*AR6jsJX5ihtNni78p5kr9A.png'>

>Focal Loss (FL) <img src='https://miro.medium.com/max/234/1*gO_nxGFmpAelOrU_D9O5-Q.png'>

> α-Balanced Variant of FL <img src='https://miro.medium.com/max/247/1*Wa6UX2I9AEtBrj5focAETA.png'>

## Model Initialization(???)

 - A prior π is set for the value of p at the start of training, so that the model’s estimated p for examples of the rare class is low, e.g. 0.01, in order to improve the training stability in the case of heavy class imbalance.
 - It is found that training RetinaNet uses standard CE loss WITHOUT using prior π for initialization leads to network divergence during training and eventually failed.
 - And results are insensitive to the exact value of π. And π = 0.01 is used for all experiments.

### RetinaNet Detector Arch
 <img src='https://miro.medium.com/max/2000/1*0-GVAp6WCzPMR6puuaYQTQ.png'>

 # Deformable Convolutional Networks.



 # Region of Interest Pooling
 <img src='https://deepsense.ai/wp-content/uploads/2017/02/roi_pooling-1.gif.pagespeed.ce.5V5mycIRNu.gif'>