---
title: Revisiting My Self Driving RC car
layout: post
summary: This post aims to improve upon the self driving rc car model in Keras.
author: Abhi Patel
date: "2020-06-28"
category:
        - guides
        - tutorials
thumbnail:
---

In 2018, I worked on a project to automate the actions of a RC car by using a feed-forward neural network to make the predictions. The input is an image of size 120x320 pixels, and the output is a one-hot encoded label for the actions left, forward, and right denoted as [1, 0, 0], [0, 1, 0], and [0, 0, 1] respectively.

This post will go over methods to improve upon the original model's 77% accuracy on test sets.

The first thing I noticed was that the keras model was 
