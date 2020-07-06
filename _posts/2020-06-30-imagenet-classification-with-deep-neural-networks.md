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

##### Summary
A large neural net was trained for the following competitions: ILSVRC-2010 and ILSVRC-2012 competitions. GPU optimization and use of 2D convolution helped speed up training. Overfitting was addressed with various methods such as dropout and . Final network architecture was 5 convolutional layers and 3 fully connected layers. Experiments suggest that results can be improved by just using larger datasets and faster GPUS.


##### Understanding The Dataset
ImageNet is dataset with 15 mill labelled images of various sizes in 22,000 categories. ImageNet Large-Scale Visual Recognition Challenge (ILSCVRC) is held every year, and the dataset used for that contains 1000 categories with around 1000 images in each category. Additionally, 50,000 are validation images, and 150,000 are test images. The metrics for performance are Top-1 and Top-5 error rates. **Top-5 error rates are the fraction of test images for which the correct label is not among the five most probable labels**. During pre-processing, images were scaled down to $256\times256$ pixels, and the mean was subtracted.


##### The Architecture
8 layers = 5 convolutional + 3 fully connected.
The last FC layer gets fed into a 1000 node softmax function which outputs probability for each of the 1000 classes.

Inputs and outputs are as follows:
$224\times224\times3$ input image
&rarr; 96 kernels of size $11\times11\times3$; stride is 4px;
&rarr; response norm + max pooling
&rarr; 256 kernels of size $5\times5\times48$
&rarr; response norm + max pooling
&rarr; 384 kernels of size $3\times3\times256$
&rarr; 384 kernels of size $3\times3\times192$
&rarr; 256 kernels of size $3\times3\times192$
&rarr; FC layer of 4095
&rarr; FC layer of 4096
&rarr; FC layer of 4096


##### REctified Linear Unit (RELU)
A function $f$ is non-saturating if $\lvert lim_{z &rarr; - \infty} f(z) = + \infty \rvert  \lor \lvert lim_{z &rarr; \infty} f(z) = + \infty \rvert$

RELU was used as the activation function because it is non-saturating unlike the sigmoid and tanh functions which are saturating. This is important because larger inputs yield a "saturated" or plateaued value such that the rate of change is low. This means that backpropagation, which uses derivatives to update weights will have smaller changes, leading to slower convergence. The RELU consistently allows the model to converge in fewer epochs compared to the tanh and sigmoid.


##### Local Response Normalization (LRN)
This technique was inspired by computational neuroscience but has fallen out of favour in current times as it provides very little error reduction. This method is used to create larger contrast between peaks and plateaus to increase recognition. See Section 3.3 on page 4 as I will not be covering the formula in detail.


##### Overlapping pooling
Max pooling is a process where matrix/tensor representations are down-sampled by the process of sampling the maximum values of selected sub-regions. Let $s$ be the increment, and $z$ be the window of interest. If $s=z$, we have local pooling which is the original method of choosing the maximum value in each sub-region without the window covering any neurons more than once. If $s<z$, then we have overlapping values where the window recaptures $z-s$ values per increment.  

"This scheme reduces the top-1 and top-5 error rates by 0.4% and 0.3%, respectively, as compared with the non-overlapping scheme $s=2$, $z=2$, which produces output of equivalent dimensions. We generally observe during training that models with overlapping pooling find it slightly more difficult to overfit" (Krizhevsky et al., 2012).


##### Dropout
Present in the first two FC layers, this technique allows the prevention of overfitting at the cost of doubling the number of iterations needed to converge. Dropout is the process of randomly assigning all neurons in a layer a probability between 0 and 1, and then "switching off" neurons that fall above a threshold - which in the case of this paper is 0.5. "Switching off" means that both forward propagation and backpropagation ignore that neuron. One quirk to remember during **test time** is that all neurons are used, but their outputs are multiplied by 0.5 to take the geometric mean.


##### Data Augmentation
Two methods of data augmentation were mentioned in the paper.
The first method is reflecting, and translating images to generate more data. $224 \times 224$ pixel patches are chosen from the $256 \times 256$ images at the four corners and in the centre. Additionally the patches are flipped along the vertical axis to generate even more images. This yields 10 times more images.

The second method utilizes Principle Component Analysis (PCA) to reduce dimensionality for each of the RGB channels. 

##### Gradient Descent, Momentum, Weight Decay

The paper states stochastic gradient descent is used, but then shortly after suggests a batch size of 128 examples. This was something I found confusing about the paper as stochastic gradient descent does not work with batches but rather single training examples. I will interpret it as mini-batch gradient descent with 128 examples per step.

This algorithm uses momentum with $\beta = 0.9$ to speed up convergence.
During gradient descent, momentum tries to damp oscillations in directions other than where the local optima is.

The problem of overfitting that plagues the model with generalizing new data is addressed with regularization. Weight decay of 0.0005 is chosen to reduce complexity.

The paper describes the following update rule: \
$$ v_{i+1} := 0.9v_{i} - 0.0005  \epsilon  w_{i} - \epsilon \langle \frac{\partial v}{\partial t} |_{w_{i}} \rangle_{_{D_{i}}} $$ \
$$ w_{i} = w_{i} + v_{i+1} $$

where:
- $i$ is the iteration index
- $v$ is the momentum variable
- $\epsilon$ is the learning rate
- $\langle \frac{\partial v}{\partial t} |_{w_{i}} \rangle_{_{D_{i}}}$ is

##### References
> Krizhevsky, Alex, Ilya Sutskever, and Geoffrey E. Hinton. "Imagenet classification with deep convolutional neural networks." Advances in neural information processing systems. 2012.
