---
title: Revisiting My Self Driving RC car
layout: post
summary: This post aims to revisit the shortcomings of the self driving rc car model in Keras.
author: Abhi Patel
date: "2020-06-28"
category: projects
thumbnail: "/assets/img/posts/side_view.jpg"
---

In 2018, I worked on a project to automate the actions of a RC car by using a feed-forward neural network to make the predictions. The input is an image of size 120x320 pixels, and the output is a one-hot encoded label for the actions left, forward, and right denoted as [1, 0, 0], [0, 1, 0], and [0, 0, 1] respectively. The accuracy is 77% on test sets.

### Data
Let's start with ways to improve the data. Initially, the height of the image was larger, but in an effort to reduce noise, I had taken the bottom portion for training such that only the road could be captured. There however, still seems to be a lot of noise that may lead the network to find useless patterns. For example, the upper corners containing the rest of the corridor, and light bouncing off the floor.

We can use image thresholding to give a cleaner separation between the road markers. An even better option is using hough transform to detect and highlight the lanes.

<div>
{% highlight python %}
# The original parse function to retrieve the images from npz files
input_data_path = glob.glob('../training_data/*.npz')

X = np.zeros((1, 38400))
y = np.zeros((1, 3), 'float')

for input_file in input_data_path:
    with np.load(input_file) as data:
        X_temp = data['train']
        y_temp = data['train_labels']

        _, X_temp = cv2.threshold(X_temp, 130, 255,cv2.THRESH_BINARY)

        # Get rid of the first rows of each training_data file
        X = np.vstack((X, X_temp[1:]))
        y = np.vstack((y, y_temp[1:]))

    cv2.threshold(X_temp, 130, 255,cv2.THRESH_BINARY)

# The first vector is empty so remove it
X = X[1:]
X = X.reshape((1071, 120, 320))

# The second vector is empty so remove it
y = y[1:]

{% endhighlight %}
</div>

![thresholded images](https://raw.githubusercontent.com/B33Boy/Self-Driving-RC-Car/master/computer/Tests/img_grid.png){:class="img-fluid"}


Next, we can shuffle our data while preserving the association between the data and labels using scikit learn's utility function.
<div>
{% highlight python %}
X, y = shuffle(X, y)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20)
{% endhighlight %}
</div>
