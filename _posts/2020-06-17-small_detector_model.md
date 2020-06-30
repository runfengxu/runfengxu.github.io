---
layout:     post
title:      Small detector model comparison
subtitle:   
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - CNN
 - Deep Learning
 - Neural Network
 - Object Detection
---
# 2015: 
## single shot detector(SSD)
## YOLO: you only look once
 ### Fast Yolo
  - 9 of 24 CNN layers from GoogleNet
  - fully connected detector network
  - real-time systems on PASCAL VOC 2007<img src='https://i.imgur.com/EixWovp.png'> 
   - 
# 2016:
## YOLO9000(v2) :Better, Faster, Stronger
 - backbone: darknet-19(19 conv layers and 5 maxpooling layers)<img src='https://i.imgur.com/3HXcYZL.png'>
# 2017:
## Fast YOLO:
 - optimzided yolo v2 reduced parameters,**evolutionary deep intelligence framework**
 - introduce a motion-adaptive-inference method.(intuitively not very reliable)
 - <img src='https://i.imgur.com/pqffNDb.png'>
# 2018:
## YOLO v3:
- backbone: Darknet-53(53 conv layers)
- 1/2 fast as YOLO v2.
## YOLO-lite
- just modify tiny yolo and see how it worked.
<img src='https://i.imgur.com/Oy3B3dB.png'>

## tiny ssd:
 - Inspired by the **Fire microarchitecture ** from SqueezeNet. First subnet stack of Tiny SSD as a standard convolutional layer followed by a set of highly optimized Fire modules.
 - determine the ideal number of Fire modules as well as the ideal microarchitecture.
 - determined empirically that 10 Fire modules provide strong object detection performance
 - feature layer:
 - result<img src="https://i.imgur.com/hutyviH.png">
 - half precision floating-point parameters.
# 2019:
## EfficientDet
 - speed: same level as yolo3<img src='https://i.imgur.com/QkCPwk1.png'>
 
 # 2020:
 ## YOLO v4:







 #THings need checking out:
 small backbone: SqueezeNet, ShuffleNet
 Two_stage detection head:
  - RCNN,fast-RCNN,faster-RCNN
  One stage detection:
  - retinaNet
Neck layers:
 - PAN,FPN,BiFPN

 - what exactly is receptive field?
