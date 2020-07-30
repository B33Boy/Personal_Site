---
title: 1x1 Convolution
layout: note
---

Note: A $1\times1$ convolution is not the same as an element-wise multiplication between the kernel and the tensor!

Suppose we have a tensor of shape  $W \times H \times D$ and a kernel of size $1 \times 1 \times D$. When the tensor is convolved with the kernel, the output will be of shape $W \times H \times 1$. This is because the kernel will first be multiplied element-wise with "same size" chunks of the tensor, and then the each chunk will be collapsed via summation (basically a dot product).

<figure>
  <img src="https://miro.medium.com/max/963/1*deVKbCzJs_7eL6p2ltkY0g.png" alt="Figure 1" width="70%" height="70%">
  <figcaption><a>Figure 1. 1x1 convolution with 1 filter gives a WxHx1 tensor</a></figcaption>
</figure>

Next, when $P$ filters are applied, we can stack the results to get an output of $W \times H \times P$. This is very useful in computer vision as now we have a means to reduce the dimensionality in the direction of image channels. If one wants to reduce dimensionality of the image width and height, then pooling layers are the way to go.

Note: In Inception networks, a ReLU nonlinearity right after each $1\times1$ convolution. The convolution is applied before convolutions with larger kernels are applied to reduce computational resources.
