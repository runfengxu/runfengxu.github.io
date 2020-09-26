---
layout:     post
title:      Gradient Clipping
subtitle:   
date:       2020-09-20
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - DNN
 - CV
 - RNN
---
https://towardsdatascience.com/what-is-gradient-clipping-b8e815cdfb48

> Grdient clipping deals with the gradient exploding problem especially in RNN neural network

### Intuition behind Exploding and Vanishing Gradients


When we train a RNN by Backpropagation Through Time, it means we first unroll the RNN in time by creating a copy of the network for each time step, viewing it as a multi-layer feedforward neural network, where the number of layers is equal to the number of time steps. Then we do backpropagation on the unrolled network, taking into account the weight sharing:
<img src='https://miro.medium.com/max/561/1*bMy_8geflaPwiiRGFdAZ_Q.png'>

where W is the recurrent weight matrix. It can be shown that the gradient of the loss function consists of a product of n copies of Wᵀ, where n is the number of layers going back in time. 

For a scaler a ≠ 1, aⁿ shrink or grow exponentially. 


### Gradient Clipping

The idea of gradient clipping is very simple: If the gradient gets too large, we rescale it to keep it small.

if |g|>c, we let $g = c*g/|g|$

where c is a hyperparameter, g is the gradient, and ‖g‖ is the norm of g.

<img src='https://miro.medium.com/max/700/1*vLFINWklJ0BtYtgzwK223g.png'>
