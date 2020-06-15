---
layout:     post
title:      Object Detection in Pytorch
subtitle:   Objection Detection Tutorial in Pytorch
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - javascript
---

source from https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/baseball.gif">

The model that are going to be implemented is SSD(single shot Multibox Detector)

Original Implementation:https://github.com/weiliu89/caffe/tree/ssd

## Concepts:
 - **Single Shot Detection**: Earlier architecture for object detection consisted of Two distinct stages- a region proposal network that performs object localization and a classifier for detection the types of objects in the proposed regions. Computationally expensive. WHile SSD excapsulate botth localization and detection tasks in a single forward sweep of the network, resulting in significantly faster detections while deployable on lighter hardware.

 - **Multiscale Feature Maps** IN classification tasks, we base our predictions onthe final convolutional feature map- the smallest but deepest. In object detection, feature maps from intermediate convolutional layers can also be directly useful because they represent the original image at different scales.

 - **Priors**: These are pre-computed boxes defined at specific positions on specific feature maps, with specific aspect ratios and scales, THey are carefully chosen to match the characteristics of objects' bounding boxes.

 - **Multibox**: This is a technique that formulates predicting an object's bounding box as a regression problem, wherein a detected object's coordinates are regressed to its ground truth's coordinates. In addition, for each predicted box, scores are generated for various object types. Priors serve as feasible starting points for predictions because they are modeled on the ground truths. Therefore, there will be as many predicted boxes as there are priors, most of whom will contain no object.

 - **Hard Negative Mining**: This refers to explicitly choosing the most egregious false positives predicted by a model and forcing it to learn from these examples. In other words, we are mining only those negatives that the model found hardest to identify correctly. In the context of object detection, where the vast majority of predicted boxes do not contain an object, this also serves to reduce the negative-positive imbalance.

 - **NonMaximum Suppression**: At any given location, multiple priors can overlap significantly. Therefore, predictions arising out of these priors could actually be duplicates of the same object. Non-Maximum Suppression (NMS) is a means to remove redundant predictions by suppressing all but the one with the maximum score.


 ## Bounding box
 - <img src='https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/bc1.PNG'>
 - <img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/bc2.PNG">
 - <img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/cs.PNG">


<hr>
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>




 ###  jaccard Index (IOU)
 measures the degree to which two boxes overlap.
<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/jaccard.jpg">


## Architecture

> **multibox** is a technique for detecting objects where a prediction consists of two components
 - Coordinates of a box that may or may not contain an object. This is a regression task
 - scores for various object types for this box, including a background class which implies there is no object in the box, This is a classification tasks
> **SSD(single shot detector)**: is purely convolutoinal neural network that we can organize into three parts
 - **Base convolutions** : derived from an existing image classification architecture that will provide lower-level feature maps.
 - **Auxiliary convolutions**: added on top of the base network that will provide higher-level feature maps.
 - **Prediction convolutions**: that will locate and identify objects in these feature maps.

 ### base convolutions- part1

 models proven to work well with image classification are already pretty good at capturing basic essence of an image. The same convolutional features are useful for object detection, There is also added advantage of being able to use layers pretrained on a reliable classification dataset. As you may know, this is called Transfering Learning.

 > vgg-16 arch: <img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/vgg16.PNG">

 > some modification to the Vgg-16:
  - The input image size will be 300, 300, as stated earlier.

  - The 3rd pooling layer, which halves dimensions, will use the mathematical ceiling function instead of the default floor function in determining output size. This is significant only if the dimensions of the preceding feature map are odd and not even. By looking at the image above, you could calculate that for our input image size of 300, 300, the conv3_3 feature map will be of cross-section 75, 75, which is halved to 38, 38 instead of an inconvenient 37, 37.

  - We modify the 5th pooling layer from a 2, 2 kernel and 2 stride to a 3, 3 kernel and 1 stride. The effect this has is it no longer halves the dimensions of the feature map from the preceding convolutional layer.

  - We don't need the fully connected (i.e. classification) layers because they serve no purpose here. We will toss fc8 away completely, but choose to rework fc6 and fc7 into convolutional layers conv6 and conv7.


### base convolutions part2
 - fc6 with a flattened input size of 7 * 7 * 512 and an output size of 4096 has parameters of dimensions 4096, 7 * 7 * 512. The equivalent convolutional layer conv6 has a 7, 7 kernel size and 4096 output channels, with reshaped parameters of dimensions 4096, 7, 7, 512.

 - fc7 with an input size of 4096 (i.e. the output size of fc6) and an output size 4096 has parameters of dimensions 4096, 4096. The input could be considered as a 1, 1 image with 4096 input channels. The equivalent convolutional layer conv7 has a 1, 1 kernel size and 4096 output channels, with reshaped parameters of dimensions 4096, 1, 1, 4096.

 The filters are numerous and computationally expensive.

 To remedy this, the authors opt to reduce the number and size of each filter by subsampling parameters from the converted convolutional layers.

  - conv6 will use 1024 filters ,each with dimension 3,3,512. the paramters are subsampled from 4096,7,7,512 to 1024,3,3,512.
  - conv7 will use 1024 filters, each with dimensions 1,1,2014. the paramters are subsampled from 4096,1,1,4096 to 1024,1,1,1024.

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/modifiedvgg.PNG">
> need to check out more about dilations:https://towardsdatascience.com/understanding-2d-dilated-convolution-operation-with-examples-in-numpy-and-tensorflow-with-d376b3972b25

### auxiliary convolutions
<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/auxconv.jpg">

## understand some procedure

### priors:

Object predictions can be quite diverse, and I don't just mean their type. They can occur at any position, with any size and shape. Mind you, we shouldn't go as far as to say there are infinite possibilities for where and how an object can occur. While this may be true mathematically, many options are simply improbable or uninteresting. Furthermore, we needn't insist that boxes are pixel-perfect.
<br>
In effect, we can discretize the mathematical space of potential predictions into just thousands of possibilities.

** Priors are precalculated, fixed boxes which collectively represent this universe of probable and approximate box predictions.
<br>
Prios are manually but carefully choosen based on the shapes and sizes of ground truth objects in out dataset. By placing these priors at every possible location in a feature map, we also account for variety in position.
<br>
 - they will be applied to various low-level and high-level feature maps, viz. those from conv4_3, conv7, conv8_2, conv9_2, conv10_2, and conv11_2. These are the same feature maps indicated on the figures before.

 - if a prior has a scale s, then its area is equal to that of a square with side s. The largest feature map, conv4_3, will have priors with a scale of 0.1, i.e. 10% of image's dimensions, while the rest have priors with scales linearly increasing from 0.2 to 0.9. As you can see, larger feature maps have priors with smaller scales and are therefore ideal for detecting smaller objects.

 - At each position on a feature map, there will be priors of various aspect ratios. All feature maps will have priors with ratios 1:1, 2:1, 1:2. The intermediate feature maps of conv7, conv8_2, and conv9_2 will also have priors with ratios 3:1, 1:3. Moreover, all feature maps will have one extra prior with an aspect ratio of 1:1 and at a scale that is the geometric mean of the scales of the current and subsequent feature map.


 <br>
 | Feature Map From | Feature Map Dimensions | Prior Scale | Aspect Ratios | Number of Priors per Position | Total Number of Priors on this Feature Map |
| :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: |
| `conv4_3`      | 38, 38       | 0.1 | 1:1, 2:1, 1:2 + an extra prior | 4 | 5776 |
| `conv7`      | 19, 19       | 0.2 | 1:1, 2:1, 1:2, 3:1, 1:3 + an extra prior | 6 | 2166 |
| `conv8_2`      | 10, 10       | 0.375 | 1:1, 2:1, 1:2, 3:1, 1:3 + an extra prior | 6 | 600 |
| `conv9_2`      | 5, 5       | 0.55 | 1:1, 2:1, 1:2, 3:1, 1:3 + an extra prior | 6 | 150 |
| `conv10_2`      | 3,  3       | 0.725 | 1:1, 2:1, 1:2 + an extra prior | 4 | 36 |
| `conv11_2`      | 1, 1       | 0.9 | 1:1, 2:1, 1:2 + an extra prior | 4 | 4 |
| **Grand Total**      |    –    | – | – | – | **8732 priors** |

There are a total of 8732 priors defined for the SSD300!
<br>
### Visualizating Priors
we defined the priors in terms of their scales and aspect ratios.

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/wh1.jpg">

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/wh2.jpg">

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/priors1.jpg">


This means that we use each prior as an approximate starting point and then find out how much it needs to be adjusted to obtain a more exact prediction for a bounding box.

So if each predicted bounding box is a slight deviation from a prior, and our goal is to calculate this deviation, we need a way to measure or quantify it.

Consider a cat, its predicted bounding box, and the prior with which the prediction was made.
<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/ecs1.PNG">

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/ecs2.PNG">


### Prediction convolutions

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/predconv1.jpg">
<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/predconv2.jpg">

reshape:

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/reshaping2.jpg">

### Multibox loss
Matching predictions to ground truths
Remember, the nub of any supervised learning algorithm is that we need to be able to match predictions to their ground truths. This is tricky since object detection is more open-ended than the average learning task.

For the model to learn anything, we'd need to structure the problem in a way that allows for comparisions between our predictions and the objects actually present in the image.

Priors enable us to do exactly this!

 - Find the Jaccard overlaps between the 8732 priors and N ground truth objects. This will be a tensor of size 8732, N.

 - Match each of the 8732 priors to the object with which it has the greatest overlap.

 - If a prior is matched with an object with a Jaccard overlap of less than 0.5, then it cannot be said to "contain" the object, and is therefore a negative match. Considering we have thousands of priors, most priors will test negative for an object.

 - On the other hand, a handful of priors will actually overlap significantly (greater than 0.5) with an object, and can be said to "contain" that object. These are positive matches.

 - Now that we have matched each of the 8732 priors to a ground truth, we have, in effect, also matched the corresponding 8732 predictions to a ground truth.

 - Let's reproduce this logic with an example.
<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/matching1.PNG">
<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/matching2.jpg">


Now, each prior has a match, positive or negative. By extension, each prediction has a match, positive or negative.

Predictions that are positively matched with an object now have ground truth coordinates that will serve as targets for localization, i.e. in the regression task. Naturally, there will be no target coordinates for negative matches.

All predictions have a ground truth label, which is either the type of object if it is a positive match or a background class if it is a negative match. These are used as targets for class prediction, i.e. the classification task.

### localization loss
We have no ground truth coordinates for the negative matches. This makes perfect sense. Why train the model to draw boxes around empty space?

Therefore, the localization loss is computed only on how accurately we regress positively matched predicted boxes to the corresponding ground truth coordinates.

Since we predicted localization boxes in the form of offsets (g_c_x, g_c_y, g_w, g_h), we would also need to encode the ground truth coordinates accordingly before we calculate the loss.

The localization loss is the averaged Smooth L1 loss between the encoded offsets of positively matched localization boxes and their ground truths.
<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/locloss.jpg">

### Confidentce loss

considering that there are usually only a handful of objects in an image, the vast majority of the thousands of predictions we made do not actually contain an object. As Walter White would say, tread lightly. If the negative matches overwhelm the positive ones, we will end up with a model that is less likely to detect objects because, more often than not, it is taught to detect the background class.

The solution may be obvious – limit the number of negative matches that will be evaluated in the loss function.

only use those predictions where the model found it hardest to recognize that there are no objects. This is called **Hard Negative Mining**.

The number of hard negatives we will use, say N_hn, is usually a fixed multiple of the number of positive matches for this image. In this particular case, the authors have decided to use three times as many hard negatives, i.e. N_hn = 3 * N_p. The hardest negatives are discovered by finding the Cross Entropy loss for each negatively matched prediction and choosing those with top N_hn losses.

Then, the confidence loss is simply the sum of the Cross Entropy losses among the positive and hard negative matches.

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/confloss.jpg">

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/totalloss.jpg">
In general, we needn't decide on a value for α. It could be a learnable parameter.

For the SSD, however, the authors simply use α = 1, i.e. add the two losses. We'll take it!


## Process Prediction

The output of the model is offsets and class scores for 8732 priors. We need to process these to obtain **final, human-interpretable bounding boxes with labels**

- for each non-background class,
     - extract the scores for this class for each of the 8732 boxes.
     - Eliminate boxes that do not meet a certrain threshold for this score.
     - The remaining boxes are candidates for this particular class of object.

**Non max Suppression**

<img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/nms1.PNG">
     - line up the candidates for each class in terms of how likely they are.
     <img src="https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/nms2.PNG">
     - draw Jaccard similarities between all the candidates in a given class.
     <img src='https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/raw/master/img/nms3.jpg'>
     - Keep the only the more likely candidate.
Algorithmatilcally:

- Upon selecting candidates for each non-background class,

    - Arrange candidates for this class in order of decreasing likelihood.

    - Consider the candidate with the highest score. Eliminate all candidates with lesser scores that have a Jaccard overlap of more than, say, 0.5 with this candidate.

    - Consider the next highest-scoring candidate still remaining in the pool. Eliminate all candidates with lesser scores that have a Jaccard overlap of more than 0.5 with this candidate.

    - Repeat until you run through the entire sequence of candidates.

<hr>
##Implementation details at https://github.com/sgrvinod/a-PyTorch-Tutorial-to-Object-Detection/blob/master/README.md

