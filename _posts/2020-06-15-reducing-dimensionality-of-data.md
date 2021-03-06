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

Encoder-decoder networks have a "bottleneck" hidden layers that compress the input representation and recover it by expanding (Hinton et al., 2006). An autoencoder is a type of encoder-decoder network where the input and output space is the same, as well as the inputs and outputs themselves. This means the purpose of the network is to memorize the general pattern of the data itself so that it can be reconstructed. Autoencoders essentially try to learn "approximations of the identity function" [2].

The problem with autoencoders comes with difficulty in optimizing weights for deeper networks. Large weight initializations lead to poor minima, and small initializations lead to tiny gradients that do not change much. This is commonly known as the exploding/vanishing gradient problem.


##### Boltzmann Machines & Pretraining
This paper proposes that stochastic binary vectors (that represent images) can be connected to stochastic binary feature detectors via symmetrically weighted connections in a restricted Boltzmann machine. A RBM is a two-layer network with inter-layer connections but no intra-layer connections. In the case presented, Boltzmann machines connect visible units $v$ (the pixels) with hidden units $h$ (the features).

The network assigns a probability to each image by an energy function: $E(v, h) = -\sum_{i \in pixels} b_{i}v_{i} - \sum_{j \in features} b_{j}h_{j} - \sum_{i, j} v_{i}h_{j}w_{ij}$

where:  
&nbsp;- $b_{i}$ and $b_{j}$ are biases  
&nbsp;- $w_{ij}$ are weights between i and j  
&nbsp;- $v_{i}$ are binary states of pixel i  
&nbsp;- $h_{j}$ are binary states of feature j  

What pretraining does is tweaks the weights and biases to decrease the value of the energy function for the original image, and increase it for images with similar features.

1. Given a training image, the binary state $h_{j}$ of each hidden unit j is set to 1 with probability defined by the sigmoid function $\sigma(x) = \frac{1}{1+\exp^{-x}}$ where $x = b_{j} + \sum_{i} v_{i}w_{ij}$

2. Upon setting binary states for each hidden unit, the binary states for the visible unit $v_{i}$ is set to 1 with the probability defined by the sigmoid function where $x = b_{i} + \sum_{j} h_{j}w_{ij}$. The paper calls this action "confabulation".

3. Next the states of the hidden units are updated via $\delta w_{ij} = \epsilon(\langle v_{i}h_{j} \rangle_{data} - \langle v_{i}h_{j} \rangle_{recon})$ where $\epsilon$ is a learning rate, $\langle v_{i}h_{j} \rangle_{data}$ is the fraction of times where both v_{i} and h_{j} are 1, and $\langle v_{i}h_{j} \rangle_{recon})$ is the remaining fraction of times confabulations occur.


The primary idea of using restricted Boltzmann machines is to take the learned output of the hidden units (which are feature detectors) and feed them into another hidden layer as visible units, and learning another time. This layer-by-layer learning allows "[e]ach layer of features to capture strong, high-order correlations between the activities of units in the layer below" (Hinton et al., 2006).

##### Unfolding and Training

After pretraining, the model is "unfolded" to fit the encoder-decoder structure by finding the decoder from the encoder. The new weights for the decoder half of the network are transposed during the unfolding process.

##### Results

The pretraining data were randomly generated curves in 2D. The pixel intensities were non-Gaussian and between 0 and 1 ensuring that the sigmoid function could be used for output units in the auto encoder. The fine-tuning stage minimizes cross-entropy error $-\sum_{i} p_{i} log(\hat{p_{i}}) - \sum_{i}(1 - p_{i})log(1 - \hat{p_{i}})$ where $p_{i}$ is intensity of pixel $i$, and $\hat{p_{i}}$ is intensity of its reconstruction.

The autoencoder is of size (28x28)-400-200-100-50-25-6 with the symmetric decoder portion reversed. When trained on 20,000 images, and tested on 10,000 new images, the autoencoder performed better than PCA. Although shallow networks can still learn to represent the input image, pretraining reduces the time it takes to train the model.

Next, a 784-1000-500-250-30 autoencoder is used to learn the digits in the MNIST dataset. Fine-tuning is done on 60,000 training images, and testing is done on 10,000 new images.  Once again, the model performs better than PCA.

The rest of the results are on page 4 of the paper.

> Hinton, Geoffrey E., and Ruslan R. Salakhutdinov. "Reducing the dimensionality of data with neural networks." science 313.5786 (2006): 504-507.  
