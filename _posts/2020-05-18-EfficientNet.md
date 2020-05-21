---
layout:     post
title:      Efficient Net
subtitle:   
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - AI
 - Deep Learning
 - CNN
---

### Depth Scaling(d): problem:
 performance not improved as expected. Vanishing Gradient.

### Width Scaling(w):
With shallow models(less deep but wider) accuracy saturates quickly with larger width.

### Resolution(r):
 - object detection: 300x300,512x512, 600x600. 
 - but accuracy gain diminishes very quickly.

### Proposed Compound Scaling:
 - $d = \alpha^{\phi}$,
 - $w = \beta^\phi$,
 - $r = \gamma^{\phi}$,
 - $such that \alpha*\beta*\gamma \approx  2$

### Efficient Architecture:
 - Given a baseline architecture.
 - Fix $\phi$ =1, assuming that twice more resources are available.
 - Try different value of $\phi$.


