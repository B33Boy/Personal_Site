---
title: Reducing the Dimensionality of Data with Neural Networks
layout: post
summary: Hinton, and Salakhutdinov's 2006 paper "Reducing the Dimensionality of Data with Neural Networks", proposes a method to find the lower dimensional representation of data by initializing the weights of an autoencoder network such that it is close to a solution. This method is stated to even outclass Principle Component Analysis (PCA).
author: Abhi Patel
date: "2020-06-24"
category: research_papers
thumbnail: "/assets/img/posts/vinland.jpg"
---

### Paper: [Hinton, and Salakhutdinov](http://www.cs.toronto.edu/~hinton/science.pdf)

Hinton, and Salakhutdinov's 2006 paper "Reducing the Dimensionality of Data with Neural Networks", proposes a method to find the lower dimensional representation of data by initializing the weights of an autoencoder network such that they are close to a solution. This method is stated to even outclass Principle Component Analysis (PCA).


The concept of dimensionality reduction is necessary to make sense of higher dimensional data that otherwise cannot be visualized or computed easily. This is done by finding the directions of greatest variance.

#### Autoencoders

Encoder-decoder networks have a "bottleneck" hidden layers that compress the input representation and recover it by expanding [1]. An autoencoder is a type of encoder-decoder network where the input and output space is the same, as well as the inputs and outputs themselves. This means the purpose of the network is to memorize the general pattern of the data itself so that it can be reconstructed. Autoencoders essentially try to learn "approximations of the identity function" [2].

The problem with autoencoders comes with difficulty in optimizing weights for deeper networks. Large weight initializations lead to poor minima, and small initializations lead to tiny gradients that do not change much. This is commonly known as the exploding/vanishing gradient problem.


##### Boltzmann Machines
This paper proposes that binary vectors (that represent images) can be modelled using a restricted Boltzmann machine that is two-layer network with inter-layer connections but no intra-layer connections. In the case presented, Boltzmann machines connect visible units $v$ (the pixels) with hidden units $h$ (the features).

The network assigns a probability to each image by an energy function: $E(v, h) = -\sum_{i \in pixels} b_{i}v_{i} - \sum_{j \in features} b_{j}h_{j} - \sum_{i, j} v_{i}h_{j}w_{ij}$

where:  
&nbsp;- $b_{i}$ and $b_{j}$ are biases  
&nbsp;- $w_{ij}$ are weights between i and j  
&nbsp;- $v_{i}$ are binary states of pixel i  
&nbsp;- $h_{j}$ are binary states of feature j  

What training does is tweaks the weights and biases to decrease the value of the energy function for the original image, and increase it for images with similar features.

1. Given a training image, the binary state $h_{j}$ of each hidden unit j is set to 1 with probability defined by the sigmoid function $\sigma(x) = \frac{1}{1+\exp^{-x}}$ where $x = b_{j} + \sum_{i} v_{i}w_{ij}$

2. Upon setting binary states for each hidden unit, the binary states for the visible unit $v_{i}$ is set to 1 with the probability defined by the sigmoid function where $x = b_{i} + \sum_{j} h_{j}w_{ij}$. The paper calls this action "confabulation".

3. Next the states of the hidden units are updated via $\delta w_{ij} = \epsilon(\langle v_{i}h_{j} \rangle_{data} - \langle v_{i}h_{j} \rangle_{recon})$ where $\epsilon$ is a learning rate, $\langle v_{i}h_{j} \rangle_{data}$ is the fraction of times where both v_{i} and h_{j} are 1, and $\langle v_{i}h_{j} \rangle_{recon})$ is the remaining fraction of times confabulations occur.


The primary idea of using restricted Boltzmann machines is to take the learned output of the hidden units (which are feature detectors) and feed them into another hidden layer as visible units, and learning another time. This layer-by-layer learning allows "[e]ach layer of features to capture strong, high-order correlations between the activities of units in the layer below" [1].



> [1] G. E. Hinton and R. R. Salakhutdinov, “Reducing the Dimensionality of Data with Neural Networks,” Science, vol. 313, no. 5786, p. 504, Jul. 2006, doi: 10.1126/science.1127647.  
[2] Ufldl.stanford.edu. n.d. Unsupervised Feature Learning And Deep Learning Tutorial. [online] Available at: <http://ufldl.stanford.edu/tutorial/unsupervised/Autoencoders/> [Accessed 27 June 2020].  
