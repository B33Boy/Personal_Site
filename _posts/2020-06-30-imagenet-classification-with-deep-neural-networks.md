---
title: ImageNet Classification with Deep Convolutional Neural Networks
layout: post
summary: Hinton, Krizhevsky, and Sutskever's 2012 paper "ImageNet Classification with Deep Convolutional Neural Networks" introduces contemporary practices in deep learning for image classification.
author: Abhi Patel
date: "2020-06-30"
category: research_papers
thumbnail: "/assets/img/posts/howls.jpg"
---

### Paper: [Hinton, Krizhevsky, and Sutskever](http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)


Notes

Who:
Hinton, Krizhevsky, and Sutskever

What:
  a large neural net was trained for the following competitions: ILSVRC-2010 and ILSVRC-2012 competitions. GPU optimization and use of 2D convolution helped speed up training. Overfitting was addressed with various methods.
  Final network architecture: 5 Conv + 3 FC layers, removing any conv layer resulted in inferior performance. Removing any conv layer results in inferior performance.

  Experiments suggest that results can be improved by just using larger datasets and faster GPUS

Why:
Attempt to increase object classification accuracy via convolutional nn architecture.


##### dataset
ImageNet is dataset with 15 mill labelled images of various sizes in 22,000 categories. ImageNet Large-Scale Visual Recognition Challenge (ILSCVRC) is held every year, and the dataset used for that contains 1000 categories with around 1000 images in each category. Additionally, 50,000 are validation images, and 150,000 are test images. The metrics for performance are Top-1 and Top-5 error rates. Top-5 error rates are the fraction of test images for which the correct label is not among the five most probable labels. Images were scaled down to 256x256 pixels, and the mean was subtracted.


##### The architecture
8 layers = 5 conv + 3 FC. The last FC layer gets fed into a 1000 node softmax function which outputs probability for each of the 1000 classes.

The first convolutional layer takes in 224x224x3 input images, computes an output with of size 11x11x3 by using 96 kernels and a stride of 4 pixels. Next response Normalization (which I won't go into), and max pooling.

The next conv layer takes in that 11x11x3 tensor and filters using 256 kernels of size 5x5x48. The next layer uses 384 kernels of size 3x3x256. The fourth layer has 384 kernels of size 3x3x192 and the final layer has 256 kernels of size 3x3x192. The fully connected layers have 4096 neurons each.

224x224x3 input image --> 11x11x3; 96 kernels of size 11x11x3; stride is 4px; --> response norm + max pooling --> 256 kernels of size 5x5x48 --> response norm + max pooling --> 384 kernels of size 3x3x256 --> 384 kernels of size 3x3x192 --> 256 kernels of size 3x3x192 --> FC layer of 4095 --> FC layer of 4096 --> FC layer of 4096


##### RELU
f is non-saturating if
$\lvert lim_{z \rarr -\infin} f(z) = + \infin \rvert  \lor \lvert lim_{z \rarr \infin} f(z) = + \infin \rvert$

RELU was used as the activation function because it is non-saturating unlike the sigmoid and tanh functions which are saturating. This is significant because larger inputs yield a "saturated" or plateaued value such that the rate of change is low. This means that backpropagation, which uses the derivative to update weights will have smaller changes, leading to slower convergence.  https://stats.stackexchange.com/questions/174295/what-does-the-term-saturating-nonlinearities-mean?rq=1

(See image on page 3)

##### Local Response Normalization
This technique was inspired by computational neuroscience but has fallen out of favour in current times as it provides very little error reduction.

##### Overlapping pooling
s is the increment size, and z is the window of interest. If s=z, we have local pooling which is taking the average/max of each window that is shifted such that no overlapping occurs. If s<z, we have overlapping values where the window captures s-z values multiple times.

"This scheme reduces the top-1 and top-5 error rates by 0.4% and 0.3%, respectively, as compared with the non-overlapping scheme s = 2, z = 2, which produces output of equivalent dimensions. We generally observe during training that models with overlapping pooling find it slightly more difficult to overfit".
https://stats.stackexchange.com/questions/283261/why-does-overlapped-pooling-help-reduce-overfitting-in-conv-nets

###### Dropout
Present in the first two FC layers, this technique allows the prevention of overfitting at the cost of doubling the number of iterations needed to converge. Dropout is the process of randomly assigning all neurons in a layer a probability between 0 and 1, and then "switching off" neurons that fall above a threshold - which in the case of this paper is 0.5. "Switching off" means that both forward propagation and backpropagation ignore that neuron. One quirk to remember during test time is that all neurons are used, but their outputs are multiplied by 0.5 to take the geometric mean.


##### Details

Stochastic gradient descent
Batch size of 128 examples
Momentum of 0.9
Weight decay of 0.0005

$v_{i+1} := 0.9v_{i} - 0.0005\dot$
