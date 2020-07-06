---
title: Revisiting My Self Driving RC car
layout: post
summary: This post aims to revisit the shortcomings of the self driving rc car model in Keras.
author: Abhi Patel
date: "2020-06-28"
category: projects
thumbnail: "/assets/img/posts/side_view.jpg"
---

In 2018, I worked on a project to automate the actions of a RC car by using a feed-forward neural network to make the predictions. The input was an image of size 120x320 pixels, and the output was a one-hot encoded label for the actions left, forward, and right denoted as [1, 0, 0], [0, 1, 0], and [0, 0, 1] respectively. The accuracy was 77% on test sets. Looking back at this project, I can see many fundamental flaws that led to this result.  

Let's start with ways to improve the data. One of the largest shortcomings I see in this project is the lack of good data collection. From the start, the method of data collection was not a good way to clearly separate input/output relationships. By this I mean that snapping an image during a button press doesn't always give a clear indication of travel, whether left, right, or straight. It also doesn't help that the quantity of data was under two thousand images (originally was approximately one thousand images). This number was doubled via the data augmentation technique of flipping images across the vertical axis. In hindsight, I could have used more data generation techniques such as slight shifts in the image.

Regarding the images themselves, the heights of the were larger originally, but in an effort to reduce noise, I had taken the bottom portion for training such that only the road could be captured. This would come with the added benefit of less computations required as there are less pixels per image. There however, still seems to be a lot of noise that may lead the network to find useless patterns. For example, the upper corners containing the rest of the corridor, and light bouncing off the floor.

We can use image thresholding to give a cleaner separation between the road markers. An even better option is using hough transform to detect and highlight the lanes.

![thresholded images](https://raw.githubusercontent.com/B33Boy/Self-Driving-RC-Car/master/computer/Tests/img_grid.png){:class="img-fluid"}

Next, we can shuffle our data while preserving the association between the data and labels using scikit learn's utility function.

<div>
{% highlight python %}
X, y = shuffle(X, y)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20)
{% endhighlight %}
</div>
