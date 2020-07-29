---
title: 1x1 Convolution
layout: note
---

Note: A $1\times1$ convolution is not the same as an element-wise multiplication between the kernel and the tensor!

Suppose we have a tensor of shape  $n \times n \times m$ and a kernel of size $1 \times 1 \times m$. When the tensor is convolved with the kernel, the output will be of shape $n \times n \times 1$. This is because the kernel will first be multiplied element-wise with "same size" chunks of the tensor, and then the each chunk will be collapsed via summation (basically a dot product).

![1x1 convolution](https://miro.medium.com/max/963/1*deVKbCzJs_7eL6p2ltkY0g.png){:class="img-fluid"}

Next, when $p$ filters are applied, we can stack the results to get an output of $n \times n \times p$. This is very useful in computer vision as now we have a means to reduce the dimensionality in the direction of image channels. If one wants to reduce dimensionality of the image width and height, then pooling layers are the way to go.


Note: In Inception networks, a ReLU nonlinearity right after each $1\times1$ convolution. The convolution is applied before convolutions with larger kernels are applied to reduce computational resources.
