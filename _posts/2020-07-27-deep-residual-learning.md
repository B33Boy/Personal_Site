---
title: Deep Residual Learning for Image Recognition
layout: post
summary: He et al.'s 2015 paper "Deep Residual Learning for Image Recognition" defines the ResNet architecture.
author: Abhi Patel
date: "2020-07-27"
category: research_papers
thumbnail: "/assets/img/posts/konosuba.png"
---


This is a classic paper that defines the ResNet architecture that has rendered deeper architectures more effective at image recognition by addressing the problem of degradation. [Degradation](https://abhipatel.netlify.app/notepad/degradation_problem/). Degradation is when accuracy of a plain deep network gets saturated, and then degrades suddenly. This goes against the preconceived logic that if the first $N$ layers can learn the data to the point of saturation, then the remaining $M$ layers should act as identity functions passing the values along until the end. However, this does not occur in practice.

The proposed solution to this is the use of residual structures within the network. The idea is that if we "pass over" and add the original input $x$ to the output of a function $F(x)$, then we can train the network such that the worst case approximation is $x$ itself. This is essentially teaching the network to learn the identity function in the case that $F(x)$ is zero.

##### Identity Mapping by Shortcuts
A residual building block is defined as: $y = F(x, {W_i}) + x$

$F(x, {W_i})$ is the residual mapping that is learned, and $x$ is the original input. In the case of two layer "pass over", $F = W_{2} \sigma (W_{1}x)$, where $w_{1}$ and $w_{2}$ are weights, and $\sigma$ is the ReLU nonlinearity. The requirement for **identity shortcuts** like the one above are that both $F(x, {W_i})$ and $x$ must be of the same dimension. If they are not the same, then **projection shortcuts** are used. Here a linear projection is applied to the input that maps it to the space spanned by $F(x, {W_{i}}))$. The output is $y = F(x, {W_i}) + W_{s}x$, where $W_{s}$ is the projection matrix.


##### Experiments: ImageNet
Out of the 4 choices of plain 18 and 34, and residual 18 and 34 layer networks, the ResNet 34 was the highest performing with a top-1 error of 25% using 10-crop testing. The degradation error has been addressed because the 34 layer ResNet has a lower training error while managing to generalize well on a validation set.

###### Preprocessing
- Image scale augmentation: 256x480x3 --> 10 images of size 224x224x3 (top left and right, bottom left and right, centre, previous transforms with horizontal flip)
- Mean subtract images
- PCA image augmentation (See ImageNet Classification with Deep Convolutional Neural Networks) to obtain images that simulate different lighting conditions.
- Randomly initialized weights

###### Implementation Details
- Batch normalization after each convolution and before activation.
- SGD mini-batch size of 256
- Dynamic learning rate starting at 0.1 divided by 10 when error plateaus
- $60 \times 10^4$ iterations trained
- Regularization hyperparam lambda of 0.0001
- momentum of 0.9
- No dropout used because heavy use of batch normalization makes it redundant.

###### ResNet 34 layer Architecture
There are three types of slight variations in ResNet 34 which are denoted A, B, and C, with C having the lowest top-1 and top-5 errors of 24.19% and 7.4% respectively. B has slightly lower top-1 and top-5 error rates than A.

A) Zero-padding shortcuts increase dimensions, all shortcuts are parameter free \
B) Projection shortcuts increase dimensions, and others are identity shortcuts \
C) Only projection shortcuts are used

The explanation for why B is slightly better than A is that zero-padded dimensions have no residual learning. Furthermore, with the extra parameters created from projection shortcuts. Although superior, the paper doesn't focus on C as the aim is to address the degradation problem, and the simpler variants achieve that.

| ResNet-34                                                                  |
|----------------------------------------------------------------------------|
|                                                                            |
| (7x7 kernel, 64 kernels, stride = 2)                                       |
|                                                                            |
| (3x3 max pool, stride = 2)                                                 |
|                                                                            |
| Repeated 3 times:<br>(3x3 kernel, 64 kernels,<br> 3x3 kernel, 64 kernels)  |
|                                                                            |
| Repeat 4 times:<br>(3x3 kernel, 128 kernels,<br>  3x3 kernel, 128 kernels) |
|                                                                            |
| Repeat 6 times<br>(3x3 kernel, 256 kernels,<br> 3x3 kernel, 256 kernels)   |
|                                                                            |
| Repeat 3 times:<br>(3x3 kernel, 512 kernels,<br> 3x3 kernel, 512 kernels)  |
|                                                                            |
| (average pool, 1000 units for FC layer, softmax function)                  |



##### References
> He, Kaiming, et al. "Deep residual learning for image recognition." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016.
