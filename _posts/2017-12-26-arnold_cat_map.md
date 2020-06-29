---
title: Arnold’s Cat Map
layout: post
summary: A method of chaotic mapping image data by applying affine transformations
author: Abhi Patel
date: '2017-12-26'
category: misc
thumbnail: "/assets/img/posts/cat.gif"
---

### Chaos
Chaos is a branch of mathematics that deals with underlying patterns within dynamical systems that appear to behave randomly. Such behaviour is displayed in real world systems such as shuffling cards, cardiac arrhythmia, planetary orbits, and fractals in nature. [1] This post is based on a type of chaotic mapping of an image.

### Arnold's Cat Map
[My Implementation In Python](https://github.com/B33Boy/Arnold-Cat-Map)

Arnold's cat map is a transformation which seems to distort an image into random patterns. The transformation,  of an n x n image in a unit square is:
$$\Gamma \, : \, (x,y) \to (2x+y,x+y) \bmod 1.$$

And in matrix notation:
$$\Gamma \left( \begin{bmatrix} x \\ y \end{bmatrix} \right) = \begin{bmatrix} 2 & 1 \\ 1 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} \bmod 1 = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 \\ 1 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} \bmod 1$$

This applies a shear in the x-direction by a factor of 1, and then a shear in the y-direction by a factor of 1, and shift of all the parts outside the unit square back into it. The mod 1 returns a value between 0 and 1.


#### The Period of The Pixel Map

The underlying pattern of this mapping is that after a number of transformations, the image cycles back into it's original state. There is no single function that shows the correlation between the side length p (in pixels), and the period Π (the number of iterations it takes to return back to the original image). However, the following observations can be made [1]:

$Π(p) = 3p$ if and only if $p = 2 · 5k$ for $k = 1, 2,....$

$Π(p) = 2p$ if and only if $p = 5k$ for $k = 1, 2,... or$

$Π(p) = 6 · 5k$ for $k = 0, 1, 2,....$

$Π(p) ≤ 12p/7$ for all other choices of p
