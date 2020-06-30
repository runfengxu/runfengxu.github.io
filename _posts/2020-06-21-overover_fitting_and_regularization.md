---
layout:     post
title:      Over Fitting and Regularization
subtitle:   
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - Machine Learning
 - Deep Learning
---

## Regularization

it basically adds the penalty as model complexity increases.

Regularization parameter(lambda) penalizes all the parameters except intercept so that model generalizes the data and won't overfit.

<img src='https://miro.medium.com/max/1044/1*-kA1uR2nBKf_1rsrKLFfkQ.png'>


### L1 Regularization
<img src='https://miro.medium.com/max/240/1*4MlW1d3xszVAGuXiJ1U6Fg.png'>

### L2 regularization:
<img src='https://miro.medium.com/max/243/1*jgWOhDiGjVp-NCSPa5abmg.png'>

The key difference between these techniques is that Lasso shrinks the less important feature’s coefficient to zero thus, removing some feature altogether. So, this works well for feature selection in case we have a huge number of features.

## SGD and Mini_batch 
### Contrasting the 3 Types of Gradient Descent

### SGD: 
Stochastic gradient descent, 

#### upsides:
 - the frequent updates immediately give an insight into the performance of the model and the rate of improvement.
 - this variant of gradient descent may be the simplest to understand and implement, especially for beginners.
 - increased model update frequency can result in faster learning on some problem
 - noisy update can allow model to avoid local minima.

#### Downsides:
 - update so frequently is more computationally expensive than other configurations.
 - the frequent updates can result in noisy gradient signal, which may cause the model parameters and model error to jump around.
 - noisy learning process down the error gradient can also make it hard for algorithm to settle.

## **Batch Gradient Descent**
batch gradient descent performs model updates at the end of each training epoch.
#### Upsides:
 - Fewer updates to the model means this variant of gradient descent is more computationally efficient.
 - decreased update frequency results in a more stable error gradient and may result in a more stable convergence
 - the separation of the calculation of prediction errors and the model update lends the algorithm to parallel processing based implementations.
 #### Downsides:
 - more stable error gradient may result in premature convergence of the model to a less optimal set of parameters.
 - the updates at the end of the training epoch require the additional complexity of accumulating prediction errors across all training examples.
 - requires the entire training set in memory and available to the algorithm.
 - model updates may be slow for large dataset.

 ## Mini-Batch gradient descent.

split training dataset into small batches that are used to calculate model error and update model coefficients.

#### upsides:
- more robust convergence, avoid local minima.
- batched updates provide a computationally more efficient process than stochastic gradient descent.
- batching allows both the efficientcy of not having all training data in memory and algorithm implementations.
#### downsides:
- mini-batch requires the configuration of an additional "mini-batch" hyperparameter.
- 