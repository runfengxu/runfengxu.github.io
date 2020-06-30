---
layout:     post
title:      Yolo
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

 - 1st, YOLO us extrmely fast. Since we frame detection as a regression problem we don't need a complex pipeline. We simply run our neural network on a new image at test time to predict detections.
 - 2nd, YOLO reasons globally about the image when making predictions. YOLO sees the entire image during training and test time so it implicitly encodes contextual information about classes as well as their appearance. YOLO makes less than harf the number of background errors compared to Fast R-CNN.
- 3rd, YOLO learns generalizable representations of objects. When trained on natural images and tested on artwork, YOLO outperforms top detection methods like DPM and R_CNN by a wide margin.

## Unified Detection

Our network uses features from the entire image to predict each bounding box. It also predicts all bounding boxes across all classes for an image simultaneously.

Our system divides the input image into an SxS grid. If the center of an object falls into a grid cell, that grid cell is responsible for detecting that object.

- Each grid cell predicts B bounding boxes and confidence scores for those boxes. confidence: $Pr(Object)*IOU_{pred}^{truth}$. If no object exists in that cell, the confidence scores should be zero. Otherwise we want the confidence score to equal the IOU. between the predicted box and ground truth.

- Each bounding box consists of 5 predictions: x, y, w, h,
and confidence. The (x, y) coordinates represent the center
of the box relative to the bounds of the grid cell. The width
and height are predicted relative to the whole image. Finally
the confidence prediction represents the IOU between the
predicted box and any ground truth box

- Each grid cell also predicts C conditional class probabilities, Pr(Classi
|Object).We only predict one set of class probabilities per grid cell, regardless of the
number of boxes B.

<img src="https://i.ibb.co/CKjmd90/1123.png" alt="1123" border="0">
<img src="https://i.ibb.co/JHHtBpy/123321.png" alt="123321" border="0">

## Network Design

- 24 convolutional layers followed by 2 fully connected layers.
<img src='https://i.ibb.co/8rMS8fW/123321.png'>

## Training


 check details in the original paper.


 ## Comparison to other real-time system
 
 - fast YOLO is the fastest on Pascal.with 52.7 % mAP.
 - YOLO 63.4% mAP.

<img src="https://i.ibb.co/7XZrgbm/123321.png" alt="123321" border="0">
<br><br>


# YOLO v2
### accuracy improvements
 - **Batch normalization**: added batch normalization in convolution layers.
 - **High-resolution classifier**:The YOLO training composes of 2 phases. First, we train a classifier network like VGG16. Then we replace the fully connected layers with a convolution layer and retrain it end-to-end for the object detection. YOLO trains the classifier with 224 × 224 pictures followed by 448 × 448 pictures for the object detection. YOLOv2 starts with 224 × 224 pictures for the classifier training but then retune the classifier again with 448 × 448 pictures using much fewer epochs. This makes the detector training easier and moves mAP up by 4%.
 - **Convolutional with Anchor Boxes**:
Here are the changes we make to the network:
Remove the fully connected layers responsible for predicting the boundary box.

We move the class prediction from the cell level to the boundary box level. Now, each prediction includes 4 parameters for the boundary box, 1 box confidence score (objectness) and 20 class probabilities. i.e. 5 boundary boxes with 25 parameters: 125 parameters per grid cell. Same as YOLO, the objectness prediction still predicts the IOU of the ground truth and the proposed box.

To generate predictions with a shape of 7 × 7 × 125, we replace the last convolution layer with three 3 × 3 convolutional layers each outputting 1024 output channels. Then we apply a final 1 × 1 convolutional layer to convert the 7 × 7 × 1024 output into 7 × 7 × 125. (See the section on DarkNet for the details.)

Using convolution filters to make predictions.
Change the input image size from 448 × 448 to 416 × 416. This creates an odd number spatial dimension (7×7 v.s. 8×8 grid cell). The center of a picture is often occupied by a large object. With an odd number grid cell, it is more certain on where the object belongs.

Remove one pooling layer to make the spatial output of the network to 13×13 (instead of 7×7).
Anchor boxes decrease mAP slightly from 69.5 to 69.2 but the recall improves from 81% to 88%. i.e. even the accuracy is slightly decreased but it increases the chances of detecting all the ground truth objects.

- Multi-Scale Training
After removing the fully connected layers, YOLO can take images of different sizes. If the width and height are doubled, we are just making 4x output grid cells and therefore 4x predictions. Since the YOLO network downsamples the input by 32, we just need to make sure the width and height is a multiple of 32. During training, YOLO takes images of size 320×320, 352×352, … and 608×608 (with a step of 32). For every 10 batches, YOLOv2 randomly selects another image size to train the model. This acts as data augmentation and forces the network to predict well for different input image dimension and scale. In additional, we can use lower resolution images for object detection at the cost of accuracy. This can be a good tradeoff for speed on low GPU power devices.

- Hierarchical classification.
 In the training of Yolo v2, it trains the end-to-end network with the object detection samples while backpropagats the classification loss from the classification  samples to train the classifier path. TO do so, they merged different dataset together using hierarchical classification.
<br><br>
 # YOLO v3
 ### class prediction
 YOLOv3 replaces the softmax function with independent logistic classifiers to calculate the likeness of the input belongs to a specific label. 

 replace the backbone to darknet-


 # YOLO v4

https://www.youtube.com/watch?v=_JzOFWx1vZg

object detectors:
 - input , backbone, neck, dense prediction, sparse prediction
 <img src="https://i.imgur.com/rD8TpUY.jpg">
 <img src="https://i.imgur.com/1J57tO7.jpg">

## training optimization
 - for backbone data augmentation
 - for detector:
     - <img src='https://i.imgur.com/m5zn7QF.jpg'>
     