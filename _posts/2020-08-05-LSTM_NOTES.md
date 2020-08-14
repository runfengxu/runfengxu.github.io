---
layout:     post
title:      LSTM and RNN
subtitle:   
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - AI
 - Deep Learning
 - CNN
 - RNN
---
## stateful and stateless LSTM
Answer from stack overflow:

Theoretically a stateless LSTM gives the same result as a statefull LSTM, but there are few pros and cons between them. 

A stateless LSTM requires you to structure your data in a particular way, in turn it is vastly more peformant, while a statefull LSTM you can have varying timesteps, but at a performance penalty.

The stateless LSTM does have state, it's just implemented differently. Instead of managing it yourself by constantly calling reset_states() your data is structure in such a way that it is automatically reset when the end of one series of timesteps is done.


