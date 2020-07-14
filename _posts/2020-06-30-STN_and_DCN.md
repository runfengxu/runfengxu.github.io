---
layout:     post
title:      STN-Spatial Transformer Network(Image Classification) and Deformable Convolution Networks
subtitle:   
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - Machine Learning
 - Deep Learning
 - Image Classification
---
# STN
https://arxiv.org/abs/1506.02025 from 2015.

STN helps to crop out and scale-normalizes the appropriate region, which can simplify the subsequent classfication task and lead to better classification performance.
<figure>
<img src='https://miro.medium.com/max/1200/1*P_nv_a_Q3LqM9d10XGE3TQ.gif'>
<img src='https://miro.medium.com/max/800/1*aXLFdbEMmRi9-bLQ7he3HQ.png' >
<figcaption>(a) Input Image with Random Translation, Scale, Rotation, and Clutter, (b) STN Applied to Input Image, (c) Output of STN, (d) Classification Prediction</figcaption></figure>

## Quick Review on Spatial Transformation Matrices
There are mainly 3 transformation learnt by STN in the paper. Indeed, more sophisticated transformation can also be applied as well.

### 1.1 Affine Transformation
<img src='https://miro.medium.com/max/944/0*HSmVuVGX7XLv-nol.png'>

<img src='https://miro.medium.com/max/1400/0*eUmX_xK2ixt4Lq4_.png'>

### 1.2 Projective Transformation
<img src='https://miro.medium.com/max/1400/1*07ai6mvHOBKyIhedj5YwHg.png'>


### 1.3 Thin Plate Spline(TPS) Transformation

<img src='https://miro.medium.com/max/1400/1*yBeuq1ViVf4McTHonosusQ.png'>

To be explored

## Spatial Transformer Network(STN)

<img src='https://miro.medium.com/max/1400/1*Nkp0wGEurUuYPmqPR0WgpA.png'>
<img src='https://miro.medium.com/max/1400/1*8rZf6SVPROUPRX4USI0oXw.png'>

### **STN = Localisation Net + Grid Generator + Sampler**

### 2.1 Localisation Net

**input** feature map: (W,H,C)

**output**: $\theta$ , parameters of transformation $T\theta$

### Grid Generator
### Sampler


## Sampling Kernel


<hr>

# **DCN**

 - Regular convolution is operated on a regular grid R.
 - Deformable convolution is operated on R but with each points augmented by a learnable offset ∆pn.
 - Convolution is used to generate 2N number of feature maps corresponding to N 2D offsets ∆pn (x-direction and y-direction for each offset).
