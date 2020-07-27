---
title: The Degradation Problem
layout: note
---

In theory, while training a deep network, the parameters should model the data to the point of saturated accuracy (i.e overfitting). Following this logic, it would make sense that deeper networks should be able to capture all the little details of the data.


<figure>
  <img src="https://miro.medium.com/max/1134/1*4fv-3pCDh2ibQK33oDVFdA.png" alt="Figure 1" width="70%" height="70%">
  <figcaption><a>Figure 1. The 56-layer network has a higher training and testing error than the 20-layer network.</a></figcaption>
</figure>

Empirical evidence shows that this is not the case (See Fig 1). Given the example of comparing training and test errors between a 20-and 56-layer network. The 20-layer network scores a lower training and testing error.

This is not because of overfitting because we see high training and testing errors on the 56-layer network. The case of overfitting would indicate low training error that should be lower than or equal to 20-layer network. This makes sense with the following intuition: if the 20-layer network learns the data to the point of saturation, then the best the 56-layer network could do is have the same parameters as the 20-layer network for the first 20 layers, and then just pass those parameters along for the next 36 layers. However, this does not happen in practice.
